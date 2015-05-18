---
layout: post
title:  "Hadoop - Let's start taming this elephant!"
---

![_config.yml]({{ site.baseurl }}/images/hadoop/hadoop-1.jpg)

This article intends to give some preliminary knowledge about the Hadoop Open Source Framework. By the end of the article you would be able to write a Hadoop Map Reduce job on a virtual machine. I will cover only the basics of Hadoop so don’t bother reading if you already know them.

I first heard the word Hadoop some time back in college but could not find any suitable online courses at that time that could help me to get started with it. Then after joining a software company I kept on hearing about this framework more and more like it was something very basic that every software engineer must know. Whenever someone in my company discussed about it I would just nod my head in agreement just to not look silly among my colleagues. My wait got over when I heard that Cloudera was offering a [Hadoop Beginner Course](https://www.udacity.com/course/intro-to-hadoop-and-mapreduce--ud617) at [Udacity](https://www.udacity.com/). If you are interested in doing the course go ahead with it. Most of my article will cover the knowledge I took away from this course.

To understand Hadoop we need to first understand some basic terms and abstractions on which Hadoop Framework is based.

#Big Data

Its difficult to define Big Data as it is a very subjective term.A reasonable definition of Big Data can be the data that is difficult to process on a single machine. For example the sales data of one store for different items may not be considered as Big Data but the sales data for all the branches of the store across the country will be considered as Big Data.

The problems with Big Data is not only the size . There are three V’s associated with Big Data that makes it difficult to process on a single machine.

1. **Volume** : The size of the data being generated.
2. **Velocity** : The rate at which the data is being generated
3. **Variety** : the different sources and formats in which the data is coming

![_config.yml]({{ site.baseurl }}/images/hadoop/hadoop-2.png)

#Map Reduce

[Map Reduce](http://en.wikipedia.org/wiki/MapReduce) is a really powerful programming model that was built by some smart guys at Google. It helps to process really large sets of data on a cluster using a parallel distributed algorithm.

Lets start learning map reduce by means of an example. Suppose I have a very large collection of documents consisting of words. I want to know how many times a particular word appear in the collection .One possible algorithm can be to create a HashMap using the Word as Keys and Count as Values and increment the count as we get different words. But as the dataset is very large it is not possible to compute this on a single machine.

**Map Reduce** model splits this collection to smaller chunks and distributes these chunks to different machines on the cluster. The user of this model expresses the computation as two functions.

**Map Function** : The map function takes an input and produces a set of intermediate key value pairs. In our example the map function will take as input the document name and its contents , will read through the contents and then emit an Intermediate Key Value pair in our case it will be (word,1).The pseudo code for the map function is shown below.

{% highlight python %}
def map(documentName, documentContent)
	for word in documentContent :
		EmitIntermediate(word, 1)
{% endhighlight %}

**Reduce Function** : The reduce function accepts an Intermediate key and a set of values for that key. It merges together these values to form a smaller set of values. The intermediate values are supplied to user’s reduce function via an iterator. In our example the reduce function will sum all the counts emitted for a particular word. The pseudo code is shown below.

{% highlight python %}
def reduce(key,values) 
      # key : a word
      # values : a list of counts
      result = 0
      for v in values:
             result += v
      Emit(result)
{% endhighlight %}

Thus map reduce converts each task to a group of map reduce functions and each map and reduce task can be performed by different machines . The results can be merged back to produce the required output.

#Hadoop

As we have learnt about some basics now it is time to talk about Hadoop.Hadoop is an open-source framework for storage and large-scale processing of datasets on community Hardware. It was developed by Doug Cutting in 2005 who was working for Yahoo at that time. He named it after his son’s toy elephant.

Hadoop was inspired by Google’s Map Reduce and Google Files System projects. The idea was to develop an open source framework where anyone can write map reduce jobs without worrying about Hardware Failures.

#HDFS

HDFS or the Hadoop Distributed File System is the file system used by Hadoop to store data among different clusters of machine. To the developers it looks like a regular file system but the Hadoop software does all the magic behind the scene to store the files in a way such that processing can be parallelised and recovery can be done in case of failures

Suppose I have a file named myData.txt which has a size of 150MB. When a file is stored in HDFS it is divided into blocks where each blocks default size is 64MB. Each block is give a unique name.

![_config.yml]({{ site.baseurl }}/images/hadoop/hadoop-3.jpeg)

Each block is stored in a different machine or node in a cluster of nodes.These nodes are called the data nodes.The information about where each block is stored is handled by another node known as the name node.The name node contains the metadata about each data node.

![_config.yml]({{ site.baseurl }}/images/hadoop/hadoop-4.jpeg)

This process may look simple but it is not as trivial as it seems because there can be a lot of hardware or software failures which can lead to loss of important data. To minimize those loses there is need to maintain some kind of redundancy of the data stored in the nodes.Hadoop replicates each block of file three times and stores it on different data nodes such that if even one node fails the data is kept secure.Now you may have a question what if my name node fails? To keep the data of the name node intact Hadoop keeps a standby name node such that even if the name node fails the stand by can take its position and the meta data is kept intact.

#Hadoop Map Reduce

Lets Explain Hadoop Map Reduce implementation with an example. Suppose I have a big file of sales data that contains the sales data of each store of Walmart in a particular year. The rows of the file looks something like this.

![_config.yml]({{ site.baseurl }}/images/hadoop/hadoop-10.png)

I need to find out the total sales of each individual store in 2012.Now as the file will contain millions of items it is not feasible to process the file serially. Map Reduce helps us to divide the file into smaller chunks, process those chunks on different machines in a cluster and then combine the results.

Instead of one machine doing the job we will have a set of machines called mappers and reducers that will help to parallelize this process.What will be the job of the mappers and the reducers? The file will be divided into smaller chunks and we will give one chunk to each mapper. The job of the mapper will be to take the chunk and separate the sales data of each store.For example if a mapper gets the above records he will make three piles namely NYC , MIAMI and LA and keep the sales data of each in the particular pile.Each reducer will be assigned a group of stores. The reducer will collect the data from the mappers of its assigned store and and sum up the values of sales of that store. For example if the first reducer is assigned NYC he will collect the NYC sales data from each mapper and sum up the values to get the total sales for NYC.Each reducer goes through his piles in alphabetical order.So the second reducer will process the sales of LA before Miami.

![_config.yml]({{ site.baseurl }}/images/hadoop/hadoop5.jpeg)

So the mappers are just programs each of which acts on a small chunk of the file. The mappers produce an intermediate key value pairs. In our case it is the store name and the Item Price. Once the mappers have done their job a phase known as Shuffle and Sort takes place. Shuffle is the movement of records from mappers to the reducers that have been assigned those records. Sort is the sorting of data by the particular reducer.The reducers get a key and a list of values.In our case the store Name and the sales data of each store. It iterates through all the values and produces the total sales result of each store in the end.

![_config.yml]({{ site.baseurl }}/images/hadoop/hadoop-6.jpeg)

#Running a Hadoop Map Reduce Job

Now its time to get hands on with Hadoop Map Reduce. Before running a Map Reduce job you need to install certain things that will help to setup the Hadoop Environment on your machine. You can download the [CDH](http://www.cloudera.com/content/cloudera/en/downloads.html) package from cloudera site and try to set up the enviroment. Or if you are lazy like me just download this [Virtual Machine](http://content.udacity-data.com/courses/ud617/Cloudera-Udacity-Training-VM-4.1.1.c.zip.) which already has the enviroment setup and run it with Virtual Box.You can find the download [instructions](https://docs.google.com/document/d/1v0zGBZ6EHap-Smsr3x3sGGpDW-54m82kDpPKC2M6uiY/edit) here.

Usually Map Reduce code is written in java. But with a feature called Hadoop Streaming you can write your mappers and reducers in any language. We will use python to write the Map reduce job.

I am assuming that you have already installed the virtual machine. Once you open the machine go to the udacity_training folder. It will have two folders code and data. The code folder contains a sample mapper and reducer program. The data folder contains some sample data you can use to test your program. It contains a file purchases.txt that contains the sales data for a dummy store. First we need to add our input to the hadoop cluster. To run any command on the Hadoop cluster we need to append it with hadoop fs.

1. Go to the code folder .
2. Put the purchases.txt file in the hadoop cluster using hadoop fs -put purchases.txt.
3. Check if the file is in the hadoop cluster using hadoop fs -ls.

![_config.yml]({{ site.baseurl }}/images/hadoop/hadoop-7.jpeg)

Now lets take a look at the mapper and reducer code.The code folder contains the mapper.py and reducer.py files. One line of the purchases.txt looks like this.

![_config.yml]({{ site.baseurl }}/images/hadoop/hadoop-11.png)

The mapper code is shown below.

{% highlight python %}
#!/usr/bin/python
# Format of each line is:
# date\ttime\tstore name\titem description\tcost\tmethod of payment
#
# We want elements 2 (store name) and 4 (cost)
# We need to write them out to standard output, separated by a tab
import sys
for line in sys.stdin:
 data = line.strip().split(“\t”)
 if len(data) == 6:
   date, time, store, item, cost, payment = data
   print “{0}\t{1}”.format(store, cost)
{% endhighlight %}

The mapper reads the chunk of the file and splits the file by using tab delimiter. Then it prints out only the store location and item cost as the output.

The reducer code is as shown.

{% highlight python %}
#!/usr/bin/python
import sys
totalSale = 0
oldKey = None
# Loop around the data
# It will be in the format key\tval
# Where key is the store name, val is the sale amount
#
# All the sales for a particular store will be presented,
# then the key will change and we’ll be dealing with the next store
for line in sys.stdin:
 data_mapped = line.strip().split(“\t”)
 if len(data_mapped) != 2:
   # Something has gone wrong. Skip this line.
   continue
 
 thisKey, thisSale = data_mapped
 
 if oldKey and oldKey != thisKey:
   print oldKey, “\t”, totalSale
   totalSale = 0
   oldKey = thisKey;
 
 oldKey = thisKey
 totalSale = totalSale + float(thisSale) 
 
if oldKey != None: 
  print oldKey, “\t”, totalSale
{% endhighlight %}

We are assuming that we are having only one reducer that will get the sorted input from the mappers. The reducer.py takes in the sorted input and keeps on checking whether the new key is equal to the previous key. When the change occurs it prints the store name and the total sales for the store.

Once you have written the mappers and reducers you can start the map reduce job using hs mapper.py reducer.py purchases.txt output.

The terminal will show variety of information like the percentage completion of job.

![_config.yml]({{ site.baseurl }}/images/hadoop/hadoop-8.jpeg)

You can also look at the information about the mappers and reducer jobs using a job tracking tool which is accessible on the browser using the URL http://localhost:50030/jobtracker.jsp.

![_config.yml]({{ site.baseurl }}/images/hadoop/hadoop-9.jpeg)

Once the processing is done you will have the output file in the hadoop fs output directory.You can also experiment changing the mapper and reducer code to calculate things like total sales for one item, total sales for all the stores.

A lot of tools have been built to make writing Hadoop Map reduce jobs easier like Hive,Pig etc. Go have a look at them if you are interested in knowing more about Hadoop.

Now Hadoop is your playground. Try doing some cool stuff using the language of your choice and do tell me about your experiences.









