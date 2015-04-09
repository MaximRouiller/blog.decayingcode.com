---
title: "How to install ElasticSearch in a Windows environment"
date: 2013-12-18 10:12:20
tags: [tool]
---

### What is ElasticSearch?

ElasticSearch is a Java-based service that will allow you to stock data and query that data. If you know about Solr, the tool is in the same spirit but with a different management model. If you don’t, you might know Lucene or Lucene.NET. ElasticSearch is a server implementation of Lucene. Among the great feature of ElasticSearch is how it reaches high availability through a proper node scaling. Mostly, you won’t have to do anything by default. Everything will work straight out of the box. 

For the full list of feature, I will redirect you to [their website](http://www.elasticsearch.org/overview/).

### System requirements

Latest Java version

### Installing Java

First, we’ll get Java out of the way. If you don’t have java installed on your machine, go to [Ninite.com](http://ninite.com/) and select the runtime “Java” and proceed with the installation.

Once this is done, you have to set your JAVA_HOME which for whatever reason is never set after installing a JRE. If you need help on how to set your environment variable for your Java JRE, [Stackoverflow is filled with such answers](http://stackoverflow.com/questions/2619584/how-to-set-java-home-on-windows-7).

To ensure that it works, open a command prompt and type “java” and press enter. It should display the help prompt for the Java command line utility.

That’s it. Now we are ready to install ElasticSearch.

### Installing ElasticSearch

First, we’ll start by going to [ElasticSearch.org](http://www.elasticsearch.org/download/) to download the latest release (0.90.8 as of this post). They release fairly often so you might want to keep yourself up to date regularly. 

After downloading the ZIP file, you will see something like this:

[![es0908_zipContent](/posts/files/es0908_zipContent_thumb.png "es0908_zipContent")](/posts/files/es0908_zipContent.png)

Inside the`bin` folder, you will have a file called `elasticsearch.bat`. Double click and you are done.

A command line will be opened and your instance will be available.

### What do we have now?

First, if you hit [http://localhost:9200/](http://localhost:9200/) with a browser, you will see a health status of your ElasticSearch instance with the version number, the name and more.

If you want to see everything that compose your ElasticSearch cluster (a cluster is defined by default even for one node), you can hit this url: [http://localhost:9200/_status?pretty=true](http://localhost:9200/_status?pretty=true)

ElasticSearch exposes a REST API that will allow you to do most action that you need on your cluster. [See the full documentation for more info.](http://www.elasticsearch.org/guide/en/elasticsearch/reference/current/index.html)&nbsp;

However, if you are from the .NET world like me, you’ll want some abstraction over that tool so that you don’t have to deal with everything at a granular level. 

### Tools

For .NET, I recommend [NEST](http://www.nuget.org/packages/NEST/) ([documentation](http://nest.azurewebsites.net/)). It provide a great abstraction with .NET 4+ support for controlling an ElasticSearch cluster.

If you want to get down to the metal, I recommend the [Sense Extension for Chrome](https://chrome.google.com/webstore/detail/sense/doinijnbnggojdlcjifpdckfokbbfpbo/details). It comes with intelisense and will allow you to do most of your maintenance through it.

### Conclusion

So I just wanted to get this out of the window. ElasticSearch is very easy to install and get started. I’m currently working on a curation tool that allows me to store lots of data from Twitter and other sources. This is what is bringing you the daily Community Updates. This tool is currently running on my station but is heavily using ElasticSearch to sort through the content at blazing speed. 

Eventually, I want to get that code out in the open. However, I know that people will question on how to get ElasticSearch ready hence why I created that post.