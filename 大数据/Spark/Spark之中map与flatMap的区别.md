map()是将函数用于RDD中的每个元素，将返回值构成新的RDD。

flatmap()是将函数应用于RDD中的每个元素，将返回的迭代器的所有内容构成新的RDD,这样就得到了一个由各列表中的元素组成的RDD,而不是一个列表组成的RDD。

有些拗口，看看例子就明白了。

~~~
val rdd = sc.parallelize(List("coffee panda","happy panda","happiest panda party"))
~~~

输入

~~~
rdd.map(x=>x).collect
~~~

结果

~~~
res9: Array[String] = Array(coffee panda, happy panda, happiest panda party)
~~~

输入

~~~
rdd.flatMap(x=>x.split(" ")).collect
~~~

结果

~~~
res8: Array[String] = Array(coffee, panda, happy, panda, happiest, panda, party)
~~~

flatMap说明白就是先map然后再flat，再来看个例子 

~~~
val rdd1 = sc.parallelize(List(1,2,3,3))
~~~

~~~
scala> rdd1.map(x=>x+1).collect
res10: Array[Int] = Array(2, 3, 4, 4)
~~~

~~~
scala> rdd1.flatMap(x=>x.to(3)).collect
res11: Array[Int] = Array(1, 2, 3, 2, 3, 3, 3)
~~~

---------------------------------------------------------------------------------------------------------------------------

点到为止版: flatMap = flatten + map; 

深坑版: 就是自函子范畴上的一个协变函子的态射函数与自然变换的组合! 

~~~
var li=List(1,2,3,4)

var res =li.flatMap(x=> x match { 

      case 3 => List(3.1,3.2) 

      case _ =>List(x*2) 

})

println(res)

  

li= List(1,2,3,4)

var res2 =li.map(x=> x match {

    case 3 =>List(3.1,3.2) 

    case _ =>x*2 

})

println(res2)

//output=>

List(2,4, 3.1,3.2, 8)

List(2,4, List(3.1,3.2), 8)

Program exited.
~~~

  

这个过程就像是先 map, 然后再将 map 出来的这些列表首尾相接 (flatten).