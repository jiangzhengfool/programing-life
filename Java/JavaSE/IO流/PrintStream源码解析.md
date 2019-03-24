## PrintStream
#### 特点
- 与其他输出流不同，PrintStream 永远不会抛出 IOException；
- 它产生的IOException会被自身的函数所捕获并设置错误标记， 用户可以通过 checkError() 返回错误标记，从而查看PrintStream内部是否产生了IOException
- 提供了自动flush 和 字符集设置功能

#### 属性
```
private final boolean autoFlush;
private boolean trouble = false;
private Formatter formatter;
private BufferedWriter textOut;// BufferedWriter对象，用于实现“PrintStream支持字符集”。
private OutputStreamWriter charOut;
```

#### 核心方法
- write
- newLine

## Reader

## BufferedReader

#### 属性
```
private Reader in;

private char cb[];
// nChars表示当前缓冲区的字符个数，nextChar表示下个可以读取的字符位置，这里命名不太好，应该命名为nextCharPos
private int nChars, nextChar;

// 与mark相关的标记，INVALIDATED表示无效的mark
private static final int INVALIDATED = -2;
// 表示没有mark，注意这里都是<0，大于等于零表示markedChar有效
private static final int UNMARKED = -1;
// 默认没有mark
private int markedChar = UNMARKED;
// 从markedChar开始可以读取的字符，包含markedChar这个位置
private int readAheadLimit = 0; /* Valid only when markedChar > 0 */

/ 是否跳过\n，用于上一个是\r的情形
/** If the next character is a line feed, skip it */
private boolean skipLF = false;

// 当调用mark时保存旧的skipLF，以便reset时恢复
/** The skipLF flag when the mark was set */
private boolean markedSkipLF = false;

private static int defaultCharBufferSize = 8192;
// 默认的StringBuffer大小
private static int defaultExpectedLineLength = 80;
```
#### 核心构造器
```
public BufferedReader(Reader in, int sz) {
    super(in);
    if (sz <= 0)
        throw new IllegalArgumentException("Buffer size <= 0");
    this.in = in;
    cb = new char[sz];
    nextChar = nChars = 0;
}
```

#### 核心方法
```
public Stream<String> lines() {
    Iterator<String> iter = new Iterator<String>() {
        String nextLine = null;

        @Override
        public boolean hasNext() {
            if (nextLine != null) {
                return true;
            } else {
                try {
                    nextLine = readLine();
                    return (nextLine != null);
                } catch (IOException e) {
                    throw new UncheckedIOException(e);
                }
            }
        }

        @Override
        public String next() {
            if (nextLine != null || hasNext()) {
                String line = nextLine;
                nextLine = null;
                return line;
            } else {
                throw new NoSuchElementException();
            }
        }
    };
    return StreamSupport.stream(Spliterators.spliteratorUnknownSize(
            iter, Spliterator.ORDERED | Spliterator.NONNULL), false);
}
```
```
// skipLF默认是false，只有readLine才会使用它，这里只要不混用read()(或者read(buf))和readLine()就不会跳过换行
// 读取一个字符，返回字符的整数形式
public int read() throws IOException {
    // 默认的锁是当前对象
    synchronized (lock) {
        ensureOpen();
        for (;;) {
            // 如果缓冲不够，则直接从底层读取，填充缓冲区，如果填充后还是没有数据，则表示读取到末尾
            if (nextChar >= nChars) {
                fill();
                if (nextChar >= nChars)
                    return -1;
            }
            // 如果是\n直接跳过，注意下一个\n会读取
            if (skipLF) {
                skipLF = false;
                if (cb[nextChar] == '\n') {
                    nextChar++;
                    continue;
                }
            }
            return cb[nextChar++];
        }
    }
}
```
```
// 读取数据到外部数组中，如果外部数组越界，直接抛出数组越界异常，注意read1不处理越界的情形。
public int read(char cbuf[], int off, int len) throws IOException {
    synchronized (lock) {
        ensureOpen();
        if ((off < 0) || (off > cbuf.length) || (len < 0) ||
                ((off + len) > cbuf.length) || ((off + len) < 0)) {
            throw new IndexOutOfBoundsException();
        } else if (len == 0) {
            return 0;
        }

        // 分多次读取，直到末尾，或者达到len，n为实际读取的个数，并返回
        int n = read1(cbuf, off, len);
        if (n <= 0) return n;
        while ((n < len) && in.ready()) {
            int n1 = read1(cbuf, off + n, len - n);
            if (n1 <= 0) break;
            n += n1;
        }
        return n;
    }
}
```
// 读取一行，分割符号为\r、\n、\r\n
String readLine(boolean ignoreLF) throws IOException {
    StringBuffer s = null;
    int startChar;

    synchronized (lock) {

        // 确保没有关闭
        ensureOpen();
        boolean omitLF = ignoreLF || skipLF;

        bufferLoop:
        for (;;) {

            if (nextChar >= nChars)
                fill();
            // 达到末尾，返回锁读取的字符串，没有读到数据则返回null
            if (nextChar >= nChars) { /* EOF */
                if (s != null && s.length() > 0)
                    return s.toString();
                else
                    return null;
            }
            boolean eol = false;
            char c = 0;
            int i;

            // 跳过上一次读取遗留的\n，上一次读取到的是\r。结合下面可以发现分隔符为\r和\n和\r\n
            // \r后第一个字符若是\n，则跳过
            /* Skip a leftover '\n', if necessary */
            if (omitLF && (cb[nextChar] == '\n'))
                nextChar++;
            // 只看第一个字符
            skipLF = false;
            omitLF = false;

            charLoop:
            // 从缓冲区读取到\n或者\r结束循环
            for (i = nextChar; i < nChars; i++) {
                c = cb[i];
                if ((c == '\n') || (c == '\r')) {
                    eol = true;
                    break charLoop;
                }
            }

            startChar = nextChar;
            // nextChar此时指向\n或\r或缓冲区末尾
            nextChar = i;

            if (eol) {
                String str;
                if (s == null) {
                    str = new String(cb, startChar, i - startChar);
                } else {
                    // 可能要遍历多次才能到达\n或\r
                    s.append(cb, startChar, i - startChar);
                    str = s.toString();
                }
                // 对于\n或\r，必须跳过
                nextChar++;
                // 注意这里设置了skipLF，也就是上一个是\r，则如果下一个是\n必须跳过
                if (c == '\r') {
                    skipLF = true;
                }
                return str;
            }
            // 如果没有找到换行符，则存到StringBuffer中
            if (s == null)
                s = new StringBuffer(defaultExpectedLineLength); // 初始化size，内容为空
            s.append(cb, startChar, i - startChar);
        }
    }
}
```

```
// 跳过n个字符不读，返回实际读取的字符个数
public long skip(long n) throws IOException {
    if (n < 0L) {
        throw new IllegalArgumentException("skip value is negative");
    }
    synchronized (lock) {
        ensureOpen();
        long r = n;
        // 跳过n个字符，r表示还有多少个字符要跳过不读
        while (r > 0) {
            if (nextChar >= nChars)
                fill();
            if (nextChar >= nChars) /* EOF */
                break;
            if (skipLF) {
                skipLF = false;
                if (cb[nextChar] == '\n') {
                    nextChar++;
                }
            }
            long d = nChars - nextChar;
            if (r <= d) {
                nextChar += r;
                r = 0;
                break;
            }
            else {
                r -= d;
                nextChar = nChars;
            }
        }
        return n - r;
    }
}
```

```
// 从底层读取数据到缓冲区的函数。注意当markedChar有效时[markedChar, nextChar)必须保留，不能覆盖
private void fill() throws IOException {
    int dst;
    // 没有标记的情况下，默认读到缓冲区0的位置，并赋值给nextChar，这也就是nextChar一直加加，却不越界的原因。
    // 缓冲数据读完了从头开始读，借助nextChar和nChars。
    if (markedChar <= UNMARKED) {
        /* No mark */
        dst = 0;
    } else {
        // 设置了markedChar，至少为0
        int delta = nextChar - markedChar;
        // 从markedChar开始读取的字符不能超过readAheadLimit上限，注意包含markedChar
        // 超过则直接失效，可以被覆盖，在调用reset会抛出异常
        if (delta >= readAheadLimit) {
            markedChar = INVALIDATED;
            readAheadLimit = 0;
            // 失效，从零开始读取，可能被覆盖
            dst = 0;
        } else {
            // 分两种情况，将[markedChar, nextChar)复制到[0, len)，然后可以覆盖的缓冲区的位置是[len, ...)
            if (readAheadLimit <= cb.length) {
                // [cb + markedChar, cb + markedChar + delta) -> [cb, cb + delta)，同时从IO读取的起始位置是delta
                System.arraycopy(cb, markedChar, cb, 0, delta);
                // markedChar从零开始
                markedChar = 0;
                dst = delta;
            } else {
                // 如果预留的位置比较大，则会扩大缓冲区，所以要注意readAheadLimit的设置
                char ncb[] = new char[readAheadLimit];
                // 比较底层的数组复制
                System.arraycopy(cb, markedChar, ncb, 0, delta);
                cb = ncb;
                markedChar = 0;
                dst = delta;
            }
            // 下一个可以读取的位置和缓冲区的个数均为delta
            nextChar = nChars = delta;
        }
    }
    int n;
    do {
        // 当dst=0时，一次读取8k
        n = in.read(cb, dst, cb.length - dst);
    } while (n == 0);
    if (n > 0) {
        // 新增的字符个数
        nChars = dst + n;
        nextChar = dst;
    }
}
```