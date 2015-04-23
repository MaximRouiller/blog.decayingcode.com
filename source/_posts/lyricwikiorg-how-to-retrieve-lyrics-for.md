---
title: "LyricWiki.org - How to retrieve lyrics for your mp3 in C#?"
date: 2009-05-06 21:29:00
tags: []
---

I have an iPhone and I got a lot of songs on it. I don't particularly love to sing but I got a few bands that have tendencies to say their lyrics in a... obscure way. Since I'm really curious, I'm always on Google searching for the lyrics. I wanted to save some time and avoid unnecessary browsing. The iPhone have the capacity of displaying lyrics if they are included inside the mp3 metadata. Updating the lyrics is something but where will I retrieved thousands song lyric?

I recently found out about a website called [LyricWiki.org](http://www.lyricwiki.org). What is interesting is that they have a web service (for free) available. I then decided to share how I did it. First thing is to start a project. You will then add a service reference like this:

[![image](http://lh6.ggpht.com/_S0YTV7NEdrk/SgI5XJ2_qaI/AAAAAAAAAEg/W0APaGWSSek/image_thumb%5B4%5D.png?imgmax=800 "image")](http://lh3.ggpht.com/_S0YTV7NEdrk/SgI5W1ZNJEI/AAAAAAAAAEc/u7WlddEqqhc/s1600-h/image%5B6%5D.png)

Click on Go, change namespace and click on OK. Once&nbsp; this is done, the easy part is done. Please note that the service URL has been stored under App.config with all the related settings.

Now to retrieve the actual lyrics, no more than a little bit of code:

```cs
// create a client to connect to the service
LyricWikiPortTypeClient client = new LyricWikiPortTypeClient();

// retrieve a song. Creed is good. Love this band.
LyricsResult song = client.getSong("Creed", "One");

// display the artist and the lyrics
Console.WriteLine(string.Format("Found lyrics for \"{0} - {1}\":\r\n\r\n" +
"{2}", song.artist, song.song, song.lyrics));

// let you read it :)
Console.ReadLine();
```

That was easy now wasn't it? Next post, how to update your mp3 with iTunes! Keep reading!
