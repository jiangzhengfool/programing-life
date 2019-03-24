## 属性

```
protected byte buf[];
protected int count; //缓冲其中的实际字节数
```

##  核心构造器
```
// size默认为8kb
public BufferedOutputStream(OutputStream out, int size) {
        super(out);
        if (size <= 0) {
            throw new IllegalArgumentException("Buffer size <= 0");
        }
        buf = new byte[size];
    }
```

## 核心方法

- public synchronized void write(int b) throws IOException
- public synchronized void write(byte b[], int off, int len) throws IOException
```