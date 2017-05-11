---
layout: post
title: Building and Adding a Runnable Spark Distrib
date:   2017-05-11
categories: scala, sbt, spark
---

If you're interested in running a version of Spark that is not yet available as a runnable distribution on [their site](), you can do the following to build your own runnable distribution and incorporate it into your `build.sbt`.

First, clone the Apache Spark repo and checkout a branch at your target release's commit. You can find Spark release tags [here](https://github.com/apache/spark/releases).

```bash
git clone https://github.com/apache/spark.git
cd spark
git checkout -b branch-name tag-name
```

Run the make-distribution script to build the runnable distribution. More details about how to do that are available [here](http://spark.apache.org/docs/latest/building-spark.html#building-a-runnable-distribution) but if you don't mind building all modules, do the following:

```bash
./dev/make-distribution.sh --name spark-vX.X.X --tgz
```

Now, in your project's root dir (`$PROJECT_ROOT`) you'll create a directory to hold the jars you just built. By default, `sbt` will look for a `lib` directory and add that to the classpath. At this point you can simply move your jars to your project's `lib` dir and you should be good to go. `sbt` should be able to resolve all dependencies successfully.

```bash
mkdir $PROJECT_ROOT/lib && mv dist/jars/* $PROJECT_ROOT/lib
```

You can however, add your own custom named directory and use that instead.

```bash
mkdir $PROJECT_ROOT/my_fav_jars && mv dist/jars/* $PROJECT_ROOT/my_fav_jars
```

To change the directory that jars are stored in, add the following line to your `build.sbt`:

```scala
unmanagedBase := baseDirectory.value / "my_fav_jars"
```

Recompile, and go!
