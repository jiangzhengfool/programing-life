## aggregate

```
public static class AvgCount implements Serializable {
    public AvgCount(int total, int num) {
        total_ = total;
        num_ = num;
    }
    public int total_;
    public int num_;
    public float avg() {
        return total_ / (float) num_;
    }
}

JavaRDD<Integer> rdd = sc.parallelize(Arrays.asList(1, 2, 3, 4));
Function2<AvgCount, Integer, AvgCount> addAndCount = new Function2<AvgCount, Integer, AvgCount>() {
    @Override
    public AvgCount call(AvgCount a, Integer x) {// 定义AvgCount加Integer的方法
    a.total_ += x;
    a.num_ += 1;
    return a;
    }
};
Function2<AvgCount, AvgCount, AvgCount> combine = new Function2<AvgCount, AvgCount, AvgCount>() {
    @Override
    public AvgCount call(AvgCount a, AvgCount b) {//定义AvgCount加AvgCount的方法
    a.total_ += b.total_;
    a.num_ += b.num_;
    return a;
    }
};
AvgCount initial = new AvgCount(0,0);
// 　aggregate函数将每个分区里面的元素进行聚合，然后用combine函数将每个分区的结果和初始值(zeroValue)进行combine操作。这个函数最终返回的类型不需要和RDD中元素类型一致。
AvgCount result = rdd.aggregate(initial, addAndCount, combine);
```

## Transformations
#### map(func)
#### filter(func)
#### flatMap(func)
 map()是将函数用于RDD中的每个元素，将返回值构成新的RDD。

flatmap()是将函数应用于RDD中的每个元素，将返回的迭代器的所有内容构成新的RDD,这样就得到了一个由各列表中的元素组成的RDD,而不是一个列表组成的RDD。
> [Spark之中map与flatMap的区别](https://blog.csdn.net/u013063153/article/details/53304087)

#### mapPartitions(func)
rdd的mapPartitions是map的一个变种，它们都可进行分区的并行处理。

两者的主要区别是调用的粒度不一样：map的输入变换函数是应用于RDD中每个元素，而mapPartitions的输入函数是应用于每个分区

另一个区别是在大数据集情况下的资源初始化开销和批处理处理，如果要初始化一个耗时的资源，然后使用，比如数据库连接。mapPartitions只需初始化3个资源（3个分区每个1次），而map要初始化10次（10个元素每个1次），显然在大数据集情况下（数据集中元素个数远大于分区数），mapPartitons的开销要小很多，也便于进行批处理操作

#### mapPartitionsWithIndex(func)
#### sample(withReplacement, fraction, seed)
#### union(otherDataset)
#### intersection(otherDataset)
交集

#### distinct([numTasks]))
#### groupByKey([numTasks])
#### reduceByKey(func, [numTasks])
> [深入理解groupByKey、reduceByKey](https://www.jianshu.com/p/0c6705724cff)

#### aggregateByKey(zeroValue)(seqOp, combOp, [numTasks])
seqOp:用来在同一个partition中合并值
combOp:用来在不同partiton中合并值
> [
Spark操作—aggregate、aggregateByKey详解](https://blog.csdn.net/u013514928/article/details/56680825)

#### sortByKey([ascending], [numTasks])
#### join(otherDataset, [numTasks])
#### cogroup(otherDataset, [numTasks])
#### cartesian(otherDataset)
笛卡尔积

#### pipe(command, [envVars])
调用外部程序

#### coalesce(numPartitions)
减少分区数量
> [Spark算子：RDD基本转换操作(2)–coalesce、repartition](http://lxw1234.com/archives/2015/07/341.htm)

#### repartition(numPartitions)
#### repartitionAndSortWithinPartitions(partitioner) 

## Actions
#### reduce(func)
#### collect()
#### count()
#### first()
#### take(n)
#### takeSample(withReplacement, num, [seed])
> [RDD Transformation——takeSample](https://blog.csdn.net/sa14023053/article/details/52016549)

#### takeOrdered(n, [ordering])
#### saveAsTextFile(path)
#### saveAsSequenceFile(path)         (Java and Scala)
#### saveAsObjectFile(path)           (Java and Scala)
#### countByValue()
#### foreach(func)
