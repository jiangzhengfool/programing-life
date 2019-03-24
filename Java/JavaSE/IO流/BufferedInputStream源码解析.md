## 属性
```
// 默认的缓冲大小是8192字节 8kb
// BufferedInputStream 会根据“缓冲区大小”来逐次的填充缓冲区；
// 即，BufferedInputStream填充缓冲区，用户读取缓冲区，读完之后，BufferedInputStream会再次填充缓冲区。如此循环，直到读完数据...
private static int defaultBufferSize = 8192;

private static int MAX_BUFFER_SIZE = Integer.MAX_VALUE - 8;

// 缓冲数组
protected volatile byte buf[];

// 缓存数组的原子更新器。
// 该成员变量与buf数组的volatile关键字共同组成了buf数组的原子更新功能实现，
// 即，在多线程中操作BufferedInputStream对象时，buf和bufUpdater都具有原子性(不同的线程访问到的数据都是相同的)
private static final
    AtomicReferenceFieldUpdater<BufferedInputStream, byte[]> bufUpdater =
    AtomicReferenceFieldUpdater.newUpdater
    (BufferedInputStream.class,  byte[].class, "buf");

// 当前缓冲区的有效字节数。
// 注意，这里是指缓冲区的有效字节数，而不是输入流中的有效字节数。
protected int count;

// 当前缓冲区的位置索引
// 注意，这里是指缓冲区的位置索引，而不是输入流中的位置索引。
protected int pos;

// 当前缓冲区的标记位置
// markpos和reset()配合使用才有意义。操作步骤：
// (01) 通过mark() 函数，保存pos的值到markpos中。
// (02) 通过reset() 函数，会将pos的值重置为markpos。接着通过read()读取数据时，就会从mark()保存的位置开始读取。
protected int markpos = -1;

// marklimit是标记的最大值。
// 关于marklimit的原理，我们在后面的fill()函数分析中会详细说明。这对理解BufferedInputStream相当重要。
protected int marklimit;
```

## 核心构造函数

```
public BufferedInputStream(InputStream in, int size) {
    super(in);
    if (size <= 0) {
        throw new IllegalArgumentException("Buffer size <= 0");
    }
    buf = new byte[size];
}
```

## 重要函数
- private void fill() throws IOException
```
// 从“输入流”中读取数据，并填充到缓冲区中。
private void fill() throws IOException
```
- public synchronized int read() throws IOException
- public synchronized int read(byte b[], int off, int len) throws IOException
- private int read1(byte[] b, int off, int len) throws IOException
```
// 将缓冲区中的数据写入到字节数组b中。off是字节数组b的起始位置，len是写入长度
private int read1(byte[] b, int off, int len) throws IOException {
   int avail = count - pos;
   if (avail <= 0) {
       // 加速机制。
       // 如果读取的长度大于缓冲区的长度 并且没有markpos，
       // 则直接从原始输入流中进行读取，从而避免无谓的COPY（从原始输入流至缓冲区，读取缓冲区全部数据，清空缓冲区， 
       //  重新填入原始输入流数据）
       if (len >= getBufIfOpen().length && markpos < 0) {
           return getInIfOpen().read(b, off, len);
       }
       // 若已经读完缓冲区中的数据，则调用fill()从输入流读取下一部分数据来填充缓冲区
       fill();
       avail = count - pos;
       if (avail <= 0) return -1;
   }
   int cnt = (avail < len) ? avail : len;
   System.arraycopy(getBufIfOpen(), pos, b, off, cnt);
   pos += cnt;
   return cnt;
}

```
    
- public void close() throws IOException

```
// 关闭输入流
public void close() throws IOException {
   byte[] buffer;
   while ( (buffer = buf) != null) {
       if (bufUpdater.compareAndSet(this, buffer, null)) {
           InputStream input = in;
           in = null;
           if (input != null)
               input.close();
           return;
       }
       // Else retry in case a new buf was CASed in fill()
   }
}
```
    
> [java io系列12之 BufferedInputStream(缓冲输入流)的认知、源码和示例](http://www.cnblogs.com/skywang12345/p/io_12.html)