---
layout: post
title:  "Inspecting Parquet Files"
date:   2017-05-09
categories: [parquet]
---

Below are the steps I took to download [parquet-tools](https://github.com/Parquet/parquet-mr/tree/master/parquet-tools), and create a command to execute it.

```bash
git clone https://github.com/Parquet/parquet-mr.git
cd parquet-tools && mvn clean package -Plocal
mkdir -p /usr/local/parquet-tools-1.6.0/jars
cp target/parquet-tools-1.6.0.jar /usr/local/parquet-tools-1.6.0/jars
cd /usr/local/parquet-tools-1.6.0
mkdir bin
touch bin/parquet-tools
echo $'#!/bin/bash\nexec java -jar /usr/local/parquet-tools-1.6.0/jars/parquet-tools-1.6.0.jar "$@"' > /usr/local/parquet-tools-1.6.0/bin/parquet-tools
chmod +x bin/parquet-tools
```

Make sure to add your parquet-tools script to your `$PATH`. Add this to your bash config:

```bash
export PATH=$PATH:/usr/local/parquet-tools-1.6.0/bin
```
