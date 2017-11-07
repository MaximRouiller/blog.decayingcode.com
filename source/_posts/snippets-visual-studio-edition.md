---
title: "Snippets: Visual Studio Edition"
date: 2016-07-12 10:00:00
tags: [snippet, visual studio]
---

While I do presentation, I'm always trying to type as few lines of code as possible.

That doesn't mean I don't want live running code on the screen however. I just don't want people seeing me fight with an API. So I do write some code in advance, figure out the right API and test it to make sure it does what I want it to do.

When it's time to present it to the public, I just have to copy/paste my code in.

But there's a better way than just copy/pasting and it's called snippets.

In this specific post, I will cover Visual Studio.

### Visual Studio Snippet Manager

So you are using Visual Studio and you want to create snippets.

First, Visual Studio ships with tons of snippets but no snippet editor. You can add/remove/import but there's no visual aid for you. So let's cover this first.

First, we have to open the snippet manager.

![Snippet Manager Menu Location](/posts/files/creating-snippets/VisualStudio-Snippet-Menu.png)

From that point, you'll be able to see all the supported languages as well as all related snippets.

![Snippet Manager](/posts/files/creating-snippets/VisualStudio-Snippet-Manager.png)

If you click on a specific snippet, you'll see where it is located. If you open that file in Notepad (or any XML editor), you'll get something like this:

```xml
<CodeSnippet Format="1.1.0" xmlns="http://schemas.microsoft.com/VisualStudio/2005/CodeSnippet">
  <Header>
    <Title>checkbox</Title>
    <Author>Meh</Author>
    <Description>None</Description>
    <Shortcut>checkbox</Shortcut> <!-- <=== this Allows you to use auto-complete -->
    <AlternativeShortcuts>
      <Shortcut Value="checkbox">asp:checkbox</Shortcut> <!-- alternative to the first shortcut that is defined -->
    </AlternativeShortcuts>
    <SnippetTypes>
      <!-- Expansion: insert at cursor and replace the typed "shortcut"  -->
      <!-- SurroundsWith: If inserted while a piece of code is selected, it will surround the selection  -->      
      <SnippetType>Expansion</SnippetType>
    </SnippetTypes>
  </Header>
  <Snippet>
    <Declarations> <!-- Here, you declare your variables that are going to be used in your snippet. -->
      <Literal>
        <ID>text</ID> <!-- Variable ID -->
        <Default>text</Default> <!-- Variable default value when inserting -->
      </Literal>
    </Declarations>
    <Code Language="html"><![CDATA[<asp:checkbox text="$text$" runat="server" />$end$]]></Code>
  </Snippet>
</CodeSnippet>
```

I've removed everything that is optional. I've only kept what is necessary to have this template functional for simplicity's sake. I've also added comments to allow you to get a better understanding of what is going on.

The code element is where you will insert your code. Everything needs to be wrapped inside a `<![CDATA[ ... ]]`. This will allow you to insert carriage return, XML or any other types of data.

If you find yourself stumped on certain snippets, check out what was already done in Visual Studio. You might find inspiration!

### Links

For more documentation on the different elements, check the [Code Snippets Schema Reference](https://msdn.microsoft.com/en-us/library/ms171418.aspx).
