1.RDD介绍：

    RDD，弹性分布式数据集，即分布式的元素集合。在spark中，对所有数据的操作不外乎是创建RDD、转化已有的RDD以及调用RDD操作进行求值。在这一切的背后，Spark会自动将RDD中的数据分发到集群中，并将操作并行化。

    Spark中的RDD就是一个不可变的分布式对象集合。每个RDD都被分为多个分区，这些分区运行在集群中的不同节点上。RDD可以包含Python，Java，Scala中任意类型的对象，甚至可以包含用户自定义的对象。

    用户可以使用两种方法创建RDD：**读取一个外部数据集，或在驱动器程序中分发驱动器程序中的对象集合，比如list或者set。**

    RDD的转化操作都是惰性求值的，这意味着我们对RDD调用转化操作，操作不会立即执行。相反，Spark会在内部记录下所要求执行的操作的相关信息。我们不应该把RDD看做存放着特定数据的数据集，而最好把每个RDD当做我们通过转化操作构建出来的、记录如何计算数据的指令列表。数据读取到RDD中的操作也是惰性的，数据只会在必要时读取。转化操作和读取操作都有可能多次执行。

2.创建RDD数据集

    （1）读取一个外部数据集

JavaRDD lines=sc.textFile(inputFile);

    （2）分发对象集合，这里以list为例

List list=new ArrayList(); list.add("a"); list.add("b"); list.add("c"); JavaRDD temp=sc.parallelize(list); //上述方式等价于  JavaRDD temp2=sc.parallelize(Arrays.*asList*("a","b","c"));

3.RDD操作

（1）转化操作

    用java实现过滤器转化操作：

List list=new ArrayList(); //建立列表，列表中包含以下自定义表项 list.add("error:a"); list.add("error:b"); list.add("error:c"); list.add("warning:d"); list.add("hadppy ending!"); //将列表转换为RDD对象 JavaRDD lines = sc.parallelize(list); //将RDD对象lines中有error的表项过滤出来，放在RDD对象errorLines中 JavaRDD errorLines = lines.filter(
        new Function, Boolean>() {
            public Boolean call(String v1) throws Exception {
                return v1.contains("error");
  }
        }
); //遍历过滤出来的列表项 List errorList = errorLines.collect(); for (String line : errorList)
    System.*out*.println(line);

输出：

error:a

error:b

error:c

可见，列表list中包含词语error的表项都被正确的过滤出来了。

（2）合并操作

将两个RDD数据集合并为一个RDD数据集

接上述程序示例：

union

输出：

error:a

error:b

error:c

warning:d

可见，将原始列表项中的所有error项和warning项都过滤出来了。

（3）获取RDD数据集中的部分或者全部元素

①获取RDD数据集中的部分元素   .take(int num)  返回值List   

获取RDD数据集中的前num项。

*/***  ** Take the first num elements of the RDD. This currently scans the partitions *one by one*, so* ** it will be slow if a lot of partitions are required. In that case, use collect() to get the* ** whole RDD instead.* **/* **def** take(num: Int): JList[T] 

程序示例：接上

JavaRDD unionLines=errorLines.union(warningLines);   for(String line :unionLines.take(2))
    System.*out*.println(line);

输出：

error:a

error:b

可见，输出了RDD数据集unionLines的前2项

②获取RDD数据集中的全部元素 .collect() 返回值 List

程序示例：

List unions=unionLines.collect(); for(String line :unions)
    System.*out*.println(line);

遍历输出RDD数据集unions的每一项

4.向spark传递函数

| 

函数名

 | 

实现的方法

 | 

用途

 |
| 

Function

 | 

R call(T)

 | 接收一个输入值并返回一个输出值，用于类似map()和filter()的操作中 |
| Function | 

R call(T1,T2)

 | 

接收两个输入值并返回一个输出值，用于类似aggregate()和fold()等操作中

 |
| FlatMapFunction | 

Iterable  call(T)

 | 

接收一个输入值并返回任意个输出，用于类似flatMap()这样的操作中

 |

 ①Function

JavaRDD errorLines=lines.filter(
        new Function, Boolean>() {
            public Boolean call(String v1)throws Exception {
                return v1.contains("error");
  }
        }
);

过滤RDD数据集中包含error的表项，新建RDD数据集errorLines

②FlatMapFunction 

List strLine=new ArrayList(); strLine.add("how are you"); strLine.add("I am ok"); strLine.add("do you love me")
JavaRDD input=sc.parallelize(strLine); JavaRDD words=input.flatMap(
        new FlatMapFunction, String>() {
            public Iterable call(String s) throws Exception {
                return Arrays.asList(s.split(" "));
  }
        }
);

将文本行的单词过滤出来，并将所有的单词保存在RDD数据集words中。

 ③ Function

List strLine=new ArrayList(); strLine.add("how are you"); strLine.add("I am ok"); strLine.add("do you love me"); JavaRDD input=sc.parallelize(strLine); JavaRDD words=input.flatMap(
        new FlatMapFunction, String>() {
            public Iterable call(String s) throws Exception {
                return Arrays.asList(s.split(" "));
  }
        }
); JavaPairRDD,Integer> counts=words.mapToPair(
        new PairFunction, String, Integer>() {
            public Tuple2, Integer> call(String s) throws Exception {
                return new Tuple2(s, 1);
  }
        }
); JavaPairRDD ,Integer> results=counts.reduceByKey(
        new org.apache.spark.api.java.function.Function2, Integer, Integer>() {
            public Integer call(Integer v1, Integer v2) throws Exception {
                return v1 + v2;
  }
        }
) ;

上述程序是spark中的wordcount实现方式，其中的reduceByKey操作的Function2函数定义了遇到相同的key时，value是如何reduce的->直接将两者的value相加。

*注意：

可以将我们的函数类定义为使用匿名内部类，就像上述程序实现的那样，也可以创建一个具名类，就像这样：

class ContainError implements Function,Boolean>{
    public Boolean call(String v1) throws Exception {
        return v1.contains("error");
  }
}
JavaRDD errorLines=lines.filter(new ContainError()); for(String line :errorLines.collect())
    System.*out*.println(line);

具名类也可以有参数，就像上述过滤出含有”error“的表项，我们可以自定义到底含有哪个词语，就像这样，程序就更有普适性了。

5.针对每个元素的转化操作：

    转化操作map()接收一个函数，把这个函数用于RDD中的每个元素，将函数的返回结果作为结果RDD中对应的元素。关键词：转化

    转化操作filter()接受一个函数，并将RDD中满足该函数的元素放入新的RDD中返回。关键词：过滤

示例图如下所示：

![](http://images2015.cnblogs.com/blog/689069/201511/689069-20151130182601702-717285451.png)

①map()

计算RDD中各值的平方

JavaRDD rdd =sc.parallelize(Arrays.asList(1,2,3,4)); JavaRDD result=rdd.map(
 new Function, Integer>() {
 public Integer call(Integer v1) throwsException {
 return v1*v1;         }
    }
); System.*out*.println( StringUtils.join(result.collect(),","));

输出：

1,4,9,16

filter()

② 去除RDD集合中值为1的元素：

JavaRDD rdd =sc.parallelize(Arrays.asList(1,2,3,4)); JavaRDD results=rdd.filter(
new Function, Boolean>() {
 public Boolean call(Integer v1) throws Exception {
 return v1!=1;         }
    }
); System.*out*.println(StringUtils.join(results.collect(),","));

结果：

2,3,4

③ 有时候，我们希望对每个输入元素生成多个输出元素。实现该功能的操作叫做flatMap()。和map()类似，我们提供给flatMap()的函数被分别应用到了输入的RDD的每个元素上。不过返回的不是一个元素，而是一个返回值序列的迭代器。输出的RDD倒不是由迭代器组成的。我们得到的是一个包含各个迭代器可以访问的所有元素的RDD。flatMap()的一个简单用途是将输入的字符串切分成单词，如下所示： 

JavaRDDString> rdd =sc.parallelize(Arrays.asList("hello world","hello you","world i love you")); JavaRDDString> words=rdd.flatMap(
 new FlatMapFunctionString, String>() {
 public IterableString> call(String s) throws Exception {
 return Arrays.asList(s.split(" "));         }
    }
); System.*out*.println(StringUtils.join(words.collect(),'\n'));

输出：

hello

world

hello

you

world

i

love

you

6.集合操作

![](http://images2015.cnblogs.com/blog/689069/201511/689069-20151130182602374-829961965.jpg)

RDD中的集合操作

| 

函数

 | 

用途

 |
| 

RDD1.distinct()

 | 生成一个只包含不同元素的新RDD。**需要数据混洗。** |
| 

RDD1.union(RDD2)

 | 返回一个包含两个RDD中所有元素的RDD |
| 

RDD1.intersection(RDD2)

 | 只返回两个RDD中都有的元素 |
| 

RDD1.substr(RDD2)

 | 返回一个只存在于第一个RDD而不存在于第二个RDD中的所有元素组成的RDD。**需要数据混洗。** |

集合操作对笛卡尔集的处理：

![](http://images2015.cnblogs.com/blog/689069/201511/689069-20151130182603921-1325319038.png)

| 

RDD1.cartesian(RDD2)

 | 返回两个RDD数据集的笛卡尔集 |

程序示例：生成RDD集合{1,2} 和{1,2}的笛卡尔集

JavaRDD rdd1 = sc.parallelize(Arrays.asList(1,2)); JavaRDD rdd2 = sc.parallelize(Arrays.asList(1,2)); JavaPairRDD,Integer> rdd=rdd1.cartesian(rdd2); for(Tuple2,Integer> tuple:rdd.collect())
    System.*out*.println(tuple._1()+"->"+tuple._2());

输出：

1->1

1->2

2->1

2->2

7.行动操作

(1)reduce操作

**    reduce()接收一个函数作为参数，这个函数要操作两个RDD的元素类型的数据并返回一个同样类型的新元素**。一个简单的例子就是函数+，可以用它来对我们的RDD进行累加。使用reduce(),可以很方便地计算出RDD中所有元素的总和，元素的个数，以及其他类型的聚合操作。

    以下是求RDD数据集所有元素和的程序示例：

JavaRDDInteger> rdd = sc.parallelize(Arrays.asList(1,2,3,4,5,6,7,8,9,10)); Integer sum =rdd.reduce(
 new Function2Integer, Integer, Integer>() {
 public Integercall(Integer v1, Integer v2) throws Exception {
 return v1+v2;         }
    }
); System.*out*.println(sum.intValue());

输出：55

(2)fold()操作

    接收一个与reduce()接收的函数签名相同的函数，再加上一个初始值来作为每个分区第一次调用时的结果。你所提供的初始值应当是你提供的操作的单位元素，也就是说，使用你的函数对这个初始值进行多次计算不会改变结果（例如+对应的0，*对应的1，或者拼接操作对应的空列表）。

    程序实例：

①计算RDD数据集中所有元素的和：

zeroValue=0;//求和时，初始值为0。

JavaRDD rdd = sc.parallelize(Arrays.asList(1,2,3,4,5,6,7,8,9,10)); Integer sum =rdd.fold(0,
 new Function2, Integer, Integer>() {
 public Integer call(Integer v1, Integer v2) throws Exception {
 return v1+v2;             }
        }
); System.*out*.println(sum);

②计算RDD数据集中所有元素的积：

zeroValue=1；//求积时，初始值为1。

JavaRDD rdd = sc.parallelize(Arrays.asList(1,2,3,4,5,6,7,8,9,10)); Integer result =rdd.fold(1,
 new Function2, Integer, Integer>() {
 public Integer call(Integer v1, Integer v2) throws Exception {
 return v1*v2;             }
        }
); System.*out*.println(result);

(3)aggregate()操作

    aggregate()函数返回值类型不必与所操作的RDD类型相同。

    与fold()类似，使用aggregate()时，需要提供我们期待返回的类型的初始值。然后通过一个函数把RDD中的元素合并起来放入累加器。考虑到每个节点是在本地进行累加的，最终，还需要提供第二个函数来将累加器两两合并。

以下是程序实例：

public class AvgCount implements Serializable{
public int total;
 public int num;
 public AvgCount(int total,int num){
 this.total=total;
 this.num=num; }
public double avg(){
 return total/(double)num; }
static Function2,Integer,AvgCount> *addAndCount*=
new Function2, Integer, AvgCount>() {
 public AvgCount call(AvgCount a, Integer x) throws Exception {
        a.total+=x;         a.num+=1;
 return a;         }
}; static Function2,AvgCount,AvgCount> *combine*=
 new Function2, AvgCount, AvgCount>() {
 public AvgCount call(AvgCount a, AvgCount b) throws Exception {
            a.total+=b.total;             a.num+=b.num;
 return a;         }
 };
 public static void main(String args[]){

        SparkConf conf = new SparkConf().setMaster("local").setAppName("my app");         JavaSparkContext sc = new JavaSparkContext(conf);           AvgCount intial =new AvgCount(0,0);         JavaRDD rdd =sc.parallelize(Arrays.asList(1, 2, 3, 4, 5, 6));         AvgCount result=rdd.aggregate(intial,*addAndCount*,*combine*);         System.*out*.println(result.avg());       }

}

这个程序示例可以实现求出RDD对象集的平均数的功能。其中addAndCount将RDD对象集中的元素合并起来放入AvgCount对象之中，combine提供两个AvgCount对象的合并的实现。我们初始化AvgCount(0,0)，表示有0个对象，对象的和为0，最终返回的result对象中total中储存了所有元素的和，num储存了元素的个数，这样调用result对象的函数avg()就能够返回最终所需的平均数，即avg=tatal/（double）num。

8.持久化缓存

    因为Spark RDD是惰性求值的，而有时我们希望能多次使用同一个RDD。如果简单地对RDD调用行动操作，Spark每次都会重算RDD以及它的所有依赖。这在迭代算法中消耗格外大，因为迭代算法常常会多次使用同一组数据。

    为了避免多次计算同一个RDD，可以让Spark对数据进行持久化。当我们让Spark持久化存储一个RDD时，计算出RDD的节点会分别保存它们所求出的分区数据。

    出于不同的目的，我们可以为RDD选择不同的持久化级别。默认情况下persist()会把数据以序列化的形式缓存在JVM的堆空间中

                                                        **不同关键字对应的存储级别表**

| 级别 | 

使用的空间

 | 

cpu时间

 | 

是否在内存

 | 

是否在磁盘

 | 

备注

 |
| 

MEMORY_ONLY

 | 高 | 

低

 | 

是

 | 

否

 | 直接储存在内存 |
| MEMORY_ONLY_SER | 

低

 | 

高

 | 

是

 | 

否

 | 

序列化后储存在内存里

 |
| 

MEMORY_AND_DISK

 | 低 | 

中等

 | 

部分

 | 

部分

 | 如果数据在内存中放不下，溢写在磁盘上 |
| 

MEMORY_AND_DISK_SER

 | 低 | 

高

 | 

部分

 | 

部分

 | 数据在内存中放不下，溢写在磁盘中。内存中存放序列化的数据。 |
| 

DISK_ONLY

 | 

低

 | 

高

 | 

否

 | 

是

 | 

直接储存在硬盘里面

 |

程序示例：将RDD数据集持久化在内存中。

JavaRDD rdd =sc.parallelize(Arrays.*asList*(1,2,3,4,5)); rdd.persist(StorageLevel.MEMORY_ONLY()); System.*out*.println(rdd.count()); System.*out*.println(StringUtils.join(rdd.collect(),',')); 

RDD还有unpersist()方法，调用该方法可以手动把持久化的RDD从缓存中移除。

9.不同的RDD类型

    Java中有两个专门的类JavaDoubleRDD和JavaPairRDD,来处理特殊类型的RDD，这两个类还针对这些类型提供了额外的函数，折让你可以更加了解所发生的一切，但是也显得有些累赘。

    要构建这些特殊类型的RDD，需要使用特殊版本的类来替代一般使用的Function类。如果要从T类型的RDD创建出一个DoubleRDD，我们就应当在映射操作中使用DoubleFunction来替代Function。

程序实例：以下是一个求RDD每个对象的平方值的程序实例，将普通的RDD对象转化为DoubleRDD对象，最后调用DoubleRDD对象的max()方法，返回生成的平方值中的最大值。

JavaRDD rdd =sc.parallelize(Arrays.*asList*(1,2,3,4,5)); JavaDoubleRDD result=rdd.mapToDouble(
 new DoubleFunction() {
 public double call(Integer integer) throws Exception {
 return (double) integer*integer;         }
    }
); System.*out*.println(result.max());

  

