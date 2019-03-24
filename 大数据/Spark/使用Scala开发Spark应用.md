

# 使用Scala开发Spark应用

## 使用sbt构建

SimpleApp.scala：

~~~
/* SimpleApp.scala */
import org.apache.spark.SparkContext
import org.apache.spark.SparkContext._
import org.apache.spark.SparkConf
object SimpleApp {
def main(args: Array[String]) {
    val logFile = "YOUR_SPARK_HOME/README.md" // 应该是你系统上的某些文 
        val conf = new SparkConf().setAppName("Simple Application")
    val sc = new SparkContext(conf)
    val logData = sc.textFile(logFile, 2).cache()
    val numAs = logData.filter(line => line.contains("a")).count()
    val numBs = logData.filter(line => line.contains("b")).count()
    println("Lines with a: %s, Lines with b: %s".format(numAs, numBs))
    }
}

~~~

我们的程序依赖于 Spark API，所以我们需要包含一个 sbt 文件文件，simple.sbt 解释了 Spark 是一个依赖。这个文件还要补充 Spark 依赖于一个 repository：

~~~
name := "Simple Project"
version := "1.0"
scalaVersion := "2.10.4"
libraryDependencies += "org.apache.spark" %% "spark-core" % "1.2.0"

~~~

目录结构:

~~~
$ find .
. .
/simple.sbt
./src
./src/main
./src/main/scala
./src/main/scala/SimpleApp.scala

~~~

打包

~~~
$ sbt package ... [info] Packaging
{..}/{..}/target/scala-2.10/simple-project_2.10-1.0.jar

~~~

使用spark-submit运行

~~~
$ YOUR_SPARK_HOME/bin/spark-submit \ --class
"SimpleApp" \ --master local[4] \ target/scala-2.10/simple-project_2.10-1.0.jar ... Lines with a:
46, Lines with b: 23

~~~

## 使用maven构建

pox.xml

~~~
scala-library
spark-core_2.10
junit
specs_2.9.3
scalatest

~~~

SimpleApp.scala

~~~
import org.apache.spark.SparkContext
import org.apache.spark.SparkContext._
import org.apache.spark.SparkConf
object SimpleApp {
def main(args: Array[String]) {
val logFile = "YOUR_SPARK_HOME/README.md" // 应该是你系统上的某些文件 val conf = new SparkConf().setAppName("Simple Application")
val sc = new SparkContext(conf)
val logData = sc.textFile(logFile, 2).cache()
val numAs = logData.filter(line => line.contains("a")).count()
val numBs = logData.filter(line => line.contains("b")).count()
println("Lines with a: %s, Lines with b: %s".format(numAs, numBs))
}
}

~~~

用spark-submit提交运行

~~~
spark-submit --class "com.spark.demo1.App" --master loca
l[2] target/spark-hello-1.0-SNAPSHOT.jar

~~~

