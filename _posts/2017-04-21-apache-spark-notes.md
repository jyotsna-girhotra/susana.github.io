---
layout: post
title:  "Apache Spark Notes"
date:   2017-04-21
categories: [apache, spark]
---


- if you're programming in Scala, this particular page in the docs regarding the [Dataset API](https://spark.apache.org/docs/latest/api/scala/index.html#org.apache.spark.sql.Dataset) will be your friend
- DataFrames are statically typed, DataSets are collections of objects
- "Dataset APIs are all expressed as lambda (anonymous) functions and JVM typed objects"
  - datasets are composed of `Dataset[T]`, strongly typed JVM objects; dataframes are composed of `Dataset[Row]`, untyped JVM objects
  - :star: [dataframes vs datasets vs RDDs](https://databricks.com/blog/2016/07/14/a-tale-of-three-apache-spark-apis-rdds-dataframes-and-datasets.html)
