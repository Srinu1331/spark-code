# spark-code
===============
dataread
===============

data = sc.textFile("file:///C://data//txns_head")

===============
filterdata
===============

filterdata= data.filter(lambda x: 'Gymnastics' in x)

data = sc.textFile("file:///C://data//txns_head")
flatdata= data.flatMap(lambda x:x.split(","))

===============
Schema Rdd
===============

schema = namedtuple("schema", ["txnno", "txndate","custid","amount","category","product","city","state","spendby"])

data = sc.textFile("file:///C://data//txns_head")
mapslit=data.map(lambda x:x.split(","))
schemardd=mapslit.map(lambda x:schema(x[0],x[1],x[2],x[3],x[4],x[5],x[6],x[7],x[8]))
schemafilter=schemardd.filter(lambda x: 'Gymnastics' in x.product)

===============
Row Rdd
===============

data = sc.textFile("file:///C://data//txns_head")
mapdata= data.map(lambda x:x.split(","))
rowdata = mapdata.map(lambda x:Row(x[0],x[1],x[2],x[3],x[4],x[5],x[6],x[7],x[8]))
filterdata = rowdata.filter(lambda x: 'Gymnastics' in str(x[5]))
filterdata.foreach(print)

===============
Schema rdd to Dataframe
===============

df = schemafilter.toDF()

===============
Row rdd to Dataframe
===============
Structype 



strct = StructType([ 
    StructField("txnno",StringType(),True), 
    StructField("txndate",StringType(),True),
    StructField("custid",StringType(),True),
    StructField("amount", StringType(), True),
    StructField("category", StringType(), True),
    StructField("product", StringType(), True),
    StructField("city", StringType(), True),
    StructField("state", StringType(), True),
    StructField("spendby", StringType(), True)
  ])

data = sc.textFile("file:///C://data//txns_head")
mapdata= data.map(lambda x:x.split(","))
rowdata = mapdata.map(lambda x:Row(x[0],x[1],x[2],x[3],x[4],x[5],x[6],x[7],x[8]))
df = spark.createDataFrame(data=rowdata,schema=strct)
df.printSchema()
df.show() 
  
  
========================================
csv read to parquet write seamless
========================================

df = spark.read.format("csv").option("header","true").load("file:///C://data//txns_head")
df.write.format("parquet").save("file:///C://data//pysparkparquet_data")
print("written done")
  
===========================
Pyspark extra jars
===========================

import os



os.environ['PYSPARK_SUBMIT_ARGS'] = '--jars C:/avrojar/* pyspark-shell'

spark = SparkSession.builder.getOrCreate()
sc = spark.sparkContext

df = spark.read.format("csv").option("header","true").load("file:///C://data//txns_head")
df.write.format("com.databricks.spark.avro").mode("overwrite").save("file:///C://data//avrowrite")
print("avro written")  


==================================
Spark Deployment
==================================
spark-submit --master local[*] --conf "spark.driver.extraClassPath=/home/cloudera/avrojar/*" Pysparkdeploy.pyss

==================================
url data
==================================
import urllib

fp = urllib.request.urlopen("https://randomuser.me/api/0.8/?results=10")
mybytes = fp.read()
mystr = mybytes.decode("utf8")
print(mystr)

rdd = sc.parallelize([mystr])
df = spark.read.json(rdd)

from pyspark.sql.functions import *
 df1=df.withColumn("results",explode(col("results"))).select("nationality","seed","version","results.user.username","results.user.cell","results.user.dob","results.user.email","results.user.gender","results.user.location.city","results.user.location.state","results.user.location.street","results.user.location.zip","results.user.md5","results.user.name.first","results.user.name.last","results.user.name.title","results.user.password","results.user.phone","results.user.picture.large","results.user.picture.medium","results.user.picture.thumbnail","results.user.registered","results.user.salt","results.user.sha1","results.user.sha256").show()

df1.show()
==================================
Spark dsl
==================================




==================================
Spark withcolumn
==================================




==================================
Spark hive
==================================

df1.write.format("parquet").saveAsTable("test.pysparkparquet")


