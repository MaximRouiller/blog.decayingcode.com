---
title: "Creating a StreamProxy with Delegate/Lambda to read/write to a file"
date: 2009-01-14 13:30:00
tags: []
---

I saw a question the last time on [Stackoverflow](http://stackoverflow.com/questions/191153/when-would-you-use-delegates-in-c "When would you use delegates in C#"). The accepted answer was from Jon Skeet which was:

*   Event handlers (for GUI and more)
*   Starting threads
*   Callbacks (e.g. for async APIs)
*   LINQ and similar (List.Find etc)
*   Anywhere else where I want to effectively apply "template" code with some specialized logic inside (where the delegate provides the specialization)

I was once asked "What is the use of a delegate?". The main answer that I found was "to delay the call". Most people see delegate as events most of the time. However, they can be put to much greater use. Here is an example that I'll gladly share with you all:

```cs
public class StreamProxy<T> where T : Stream
{
    private Func<T> constructorFunction;

    private StreamProxy(Func<T> constructor)
    {
        constructorFunction = constructor;
    }

    public void Write(Action<StreamWriter> func)
    {
        using(T stream = constructorFunction())
        {
            StreamWriter streamWriter = new StreamWriter(stream);
            func(streamWriter);
            streamWriter.Flush();
        }
    }

    public String Read(Func<StreamReader, String> func)
    {
        using (T stream = constructorFunction())
        {
            string result = func(new StreamReader(stream));
            return result;
        }
    }

    public static StreamProxy<T> Create(Func<T> func)
    {
        return new StreamProxy<T>(func);
    }
}
```

To summarize what it does... it accept a delegate that returns a class that derives from "Stream" that will be used as a Constructor. It will return you a **StreamProxy** object that you can then use to read or write String out of that stream.          What is interesting is that when it's first created... nothing is done on the file. You are just giving instruction to the class on how to access it.          When you then read/write from the file, the class knows how to manage a stream and make sure that no locks are left on the files.          

Here is a sample usage of that class:

```cs
// Here I use a FileStream but it can also be a MemoryStream or anything that derives from Stream
StreamProxy<FileStream> proxy = StreamProxy<FileStream>.Create(() => new FileStream(@"C:\MyTest.txt", FileMode.OpenOrCreate));

// Writing to a file
proxy.Write(stream => stream.WriteLine("I am using the Stream Proxy!"));

// Reading to a file
string contentOfFile = proxy.Read(stream => stream.ReadToEnd());
```

That's all folks! As long as you can give a Stream to this proxy, you won't need to do any "using" in your code and everything will stay clean!

See you all next time!