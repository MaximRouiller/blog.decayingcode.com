---
title: "Snippets: Atom Edition"
date: 2016-07-12 10:30:00
tags: [snippet, atom]
---

To continue on my previous post about [Visual Studio Snippets](/post/snippets-visual-studio-edition), I thought it might be nice to also do Atom.

First, to open your snippets go in `File > Snippets...`.

![Atom Snippet Menu Location](/posts/files/creating-snippets/atom-snippet-menu.png)

This will open up a `CSON` file.

### CSON File format

Think of it as JSON but with comments and instead of using angular brackets, it uses indentation to define sub-elements.

Another thing that is very important to note, single `'` or double `"` don't matter.

Finally, is you want to do multi-lines snippet (very useful!), you'll need to triple those quotes.

Here's a sample valid CSON

```coffeescript
'rootObject':
    'child object':
        "another something": "value of thing"
        'property': '''
        multiple line property
        '''
        "anotherMultilineProperty": """
        Having lots of fun
        over here
        """
```

So? Familiar with the format?

Let's create our first snippet.

### First snippet

So let's start with creating a snippet for a basic text file.

```coffeescript
'.text.plain': # Scope of the snippet
    'Title of my snippet': # Title
        'prefix': 'copyright' # What text will trigger the snippet
        'body': '© Novaweb Solutions' # Content of the snippet
```

Same thing but with multi-lines support:

```coffeescript
'.text.plain': # Scope of the snippet
    'Title of my snippet': # Title
        'prefix': 'copyright' # What text will trigger the snippet
        'body': '''
        Other legalese here?
        © Novaweb Solutions
        '''
```

Wow. That was easy. But what about other file-type?

### Scopes

Atom have a more stylish way of scoping snippets that works across languages and filetypes. Including support for files that are included by installing packages.

Those are scope. When a file is parsed, it will be tokenized and scopes will be added depending on your cursor location.

Every file has a scope that is global. The default one is `text.plain` (translated to `.text.plain` for CSON purposes).

So let's say I have this JavaScript file and I want to know what it's scope is.

If you open the file and run the command `Editor: Log Cursor Scope` (CTRL-ALT-SHIFT-P), a popup will show you the current scope.

![Atom JavaScript Scope](/posts/files/creating-snippets/atom-snippet-scope.png)

Now, you just need to replace `.text.plain` by `.source.js` and your snippet will be active for that file type.

If you want to have a snippet active for all source code files, you could also use `.source`.

### Variables in templates

Well, there is no real variables. You can however tokenize areas where you will be prompted for value.

Let's take our previous template and tokenize it!

```coffeescript
'.text.plain': # Scope of the snippet
    'Title of my snippet': # Title
        'prefix': 'copyright' # What text will trigger the snippet
        'body': '© ${1: Company Name}' # Content of the snippet
```


Just remember that you can't have more than one scope present at the same time in your CSON file. All other snippets must go under that scope.

Snippets is just one of the many ways to avoid retyping the same code over and over. If you are interested, share your favorite snippet in the comments bellow! 
