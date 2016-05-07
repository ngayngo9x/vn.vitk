# README #

This is the first release of a Vietnamese text processing toolkit,
which is called "ViTk", developed by Phuong LE-HONG at College of
Natural Sciences, Vietnam National University, Hanoi.

There are some toolkits for Vietnamese text processing which are
already published. However, most of them are not readily scalable for
large data processing. This toolkit aims at the ability of processing
big textual data. For this reason, it uses Apache Spark as its core
platform. Apache Spark is a fast and general engine for large
scale data processing. And hence, ViTk is a fast cluster computing
toolkit.

As an example, the Vietnamese word segmentation tool of ViTk can
tokenize a text of two million Vietnamese syllables in less than two
minutes on a cluster of three computers (24 cores, 24 GB RAM), giving an
accuracy of about 97%.

## Tools ##

Currently, ViTk consists of two fundamental tools:

* Word segmentation
* Dependency parsing 

The word segmentation tool is specific to the Vietnamese language. The
dependency parser is general and can be trained to parse any
language; we have tested with parsing English or Vietnamese.

We are working to develop and integrate more fundamental tools to
ViTk, including part-of-speech tagging, a named entity recognition,
etc. We hope that they will be available soon. 

## Setup and Compilation ##

* Prerequisites: A Java Development Kit (JDK), version 7.0 or
  above (http://www.oracle.com/technetwork/java/javase/downloads/index.html).
	Apache Maven version 3.0 or above (http://maven.apache.org/). Make
  sure that two following commands work perfectly in your shell
  (console window).

	java -version
	mvn -version

* Download Apache Spark (https://spark.apache.org/).
	ViTk uses Spark version 1.6.1. Unpack the tarball to a directory,
	for example "~/spark" where "~" is your home directory.

* Download ViTk, either a binary archive or its source code
  The source code version is preferable. It is easy to compile and
  package ViTk: go to the top-level directory of ViTk and invoke the
  following command at a shell window:

	mvn compile package

	Apache Maven will automatically resolve and download dependency
	libraries required by ViTk. Once the process finishes, you should
	have a binary jar file "vn.vitk-1.0.jar" in the subdirectory
	"target". 


## Running ##

### Resource Files Preparation ###

By default, the main tool of ViTk is Vietnamese word
segmentation. This tool uses some data files specified in the
subdirectory "dat/tok" and "dat/whitepace.model". If you run ViTk, by
default it accepts a source text and tokenizes the text into
tokens. 

ViTk can run as an application on a stand-alone cluster mode  or on a
real cluster. Since the toolkit runs on a cluster, it is required that
all machines in the cluster are able to access the same data files,
which are normally located in a shared directory readable by all the
machines.

If you use a Linux operating system, it is easy to share or
"export" a directory via a network file system (NFS). By default, ViTK
searches for data files in the directory "/export/dat/". Therefore, you need
to copy subdirectories "dat/tok" and "dat/tok/whitespace.model" into that
directory, so you have two data folders:

* /export/dat/tok
* /export/dat/tok/whitespace.model

If you run ViTk on a stand-alone cluster mode, it is sufficient to
create the two data folders specified above. The NFS stuffs can be
ignored. 

### Usage ###

ViTk is an Apache Spark application, you can run it by submitting the
main JAR file "vn.vitk-1.0.jar" to Apache Spark.

The parameters of ViTk are as follows:

* -m <master-url>: the master URL for the cluster, for example
  spark://192.168.1.1:7077. If you do not have a cluster, you can
  ignore this parameter. In this case, ViTk uses the stand-alone
  cluster mode, which is defined by "local[*]", that is it uses all
  the CPU core of your single machine.

* -i <input-file>: the name of an input file to be segmented. This
   should be a text file in UTF-8 encoding. ViTK will read and
   tokenize every lines of this file.

* -u <url>: an Internet URL containing Vietnamese text to be
   segmented. This is normally an URL to an electronic newspaper, for
   example "http://vneconomy.vn/thoi-su/bo-cong-thuong-len-tieng-ve-sieu-du-an-tren-song-hong-20160507072218349.htm".
	 If this parameter is used, ViTk will extract the main text content
   of the article and tokenize it, line by line. Note that either
   parameter -i or -u should be used.  

* -o <output-file>: the name of an output file containing the
   tokenization results. Since by default, ViTk uses a Hadoop file
   system when saving results, this is actually a directory containing
   resulting text files. If this parameter is not specified, the result is
   printed on the console window.

* -v: this parameter does not require argument. If it is used, ViTk
   runs in the verbose mode, in which some intermediate information
   will be printed out during the processing.
	
* -s: this parameter does not require argument. If it is used, ViTk
   uses a logistic regression model to disambiguate the space
   character when segmenting Vietnamese phrases instead of using a
   phrase graph as default. The model is
   pre-trained and loaded from the data directory "whitespace.model".
   However, the result is often worse than the default. Thus, you should
   consider to use this option as an extra experimentation.

Suppose that Apache Spark has been installed in "~/spark", ViTk has
been installed in "~/vitk", data files have been copied to
"/export/dat/tok". To launch ViTk, open a console, enter "~/spark"
and invoke an appropriate command according to your need. For example:

*	./bin/spark-submit ~/vitk/target/vn.vitk-1.0.jar -u <url>

* ./bin/spark-submit ~/vitk/target/vn.vitk-1.0.jar -i <input-file>

* ./bin/spark-submit ~/vitk/target/vn.vitk-1.0.jar -i <input-file> -o <output-file>

* ./bin/spark-submit ~/vitk/target/vn.vitk-1.0.jar -m <master-url> -u <url>

* ./bin/spark-submit ~/vitk/target/vn.vitk-1.0.jar -m <master-url> -i <input-file> -o <output-file>

* ./bin/spark-submit ~/vitk/target/vn.vitk-1.0.jar -m <master-url> -i  <input-file> -o <output-file> -v


## Contribution Guidelines ##

* Writing tests
* Code review
* Contributions

## Contact ##

Any bug reports, suggestions, collaborations are welcome. You can
reach me at: 

* LE-HONG Phuong, <lhphuong@protonmail.ch>
* http://mim.hus.vnu.edu.vn/phuonglh/vitk
