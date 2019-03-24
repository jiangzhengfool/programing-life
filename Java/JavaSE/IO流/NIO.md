## bio、nio和aio的区别、三种IO的用法与原理

## NIO模型，select/epoll的区别，多路复用的原理

## NIO的好处，Netty线程模型，什么是零拷贝

## 使用NIO进行快速的文件拷贝
~~~
FileChannel inChannel = new FileInputStream(in).getChannel();
    FileChannel outChannel = new FileOutputStream(out).getChannel();
    try {
        // inChannel.transferTo(0,inChannel.size(),outChannel);//original--apparentlyhastroublecopyinglargefilesonWindows
        // magicnumberforWindows,64Mb-32Kb)
        int maxCount = (64 * 1024 * 1024) - (32 * 1024);
        long size = inChannel.size();
        long position = 0;
        while (position < size) {
            position += inChannel.transferTo(position, maxCount, outChannel);
        }
    } finally {
        if (inChannel != null) {
            inChannel.close();
        }
        if (outChannel != null) {
            outChannel.close();
        }
    }
}
~~~