---
title: Content In HTML? Compile it from Markdown instead.
date: 2015-04-09 15:07:00
tags: []
---
Most content on our blogs, CMS or anything relating to content input is, today, created with [WYSIWYG][1] editors. Those are normally in a browser but can also be found in a desktop application. 

### Browser version

Libraries like [Bootstrap WYSIWYG][2] and [TinyMCE][3] leverage your browser to generate HTML that isn't particularly stylized (no CSS classes or styles) instead relying on the webpage style to render properly. Those by itself are not too complicated. However, they are enclosed in the HTML semantic at the moment of writing. 

When writing your content in a CMS, it will be stored as-is and re-rendered with almost the exact same HTML that you created at first (some sanitize your input and sometimes append content). 

### Problem for code-based blog

Most blogs devoted to blogging will contain some code at a certain point. This is where stuff starts to smell. Most WYSIWYG editors will generate the code and wrap it into `pre`/`code` or both tags. Then it will have a `class` attribute that will be tied to the code generated at the moment. 

My blog has been migrated multiple times. At first I was on Blogger, then on BlogEngine.NET and finally on MiniBlog. All those engines stored the code as it was written with the editor. Worse even if I used Live Writer since it will not strip `style` attributes and other nonsense. At best, you end-up with some very horrible HTML that you have to clean in between export/import. At worse, there is some serious issue with how your post are rendered.

The content is rendered as it was written but not as it was meant to appear. Writing code without Live Writer? Well, you'll need to write some HTML and don't forget the proper tags and correct CSS class!

### Content as source code

I see my content as source code. It should be a in a standard format that can be compiled to HTML upon my need. Did my Syntax Highlighter plugin changed and I now need to re-render my code tags? I want to just toggle some options. Not re-go through all my content doing Find & Replace. 

I want to manage my content like I manage my code. A bug? Pull Request. Typo? Pull Request.

That is why my blog is going to end-up in a GitHub repository very soon. Easier to correct things.

### But why not HTML?

HTML as well as it's "human readable" really is not. Once you start creating complex content with it, you need a powerful editor to write HTML. Nothing that can be done in Notepad.

Writing a link in Markdown is something like this:

```md
[My Blog](http://blog.maximerouiller.com)
```

And in HTML it goes to this:

```html
<a href="http://blog.maximerouiller.com" alt="">My Blog</a>
```

That's why I think that a format like [Markdown][4] is the way to go. You write the semantic you want and let the markdown renderer generate the proper HTML. Markdown is the source. HTML is the compilation.

Do I want to generate an EPUB instead? You can. The [ProGit book][5] is completely written in Markdown.

Do I want to generate a PDF instead? You also can. In fact, [Pandoc][6] supports a lot more. It got you covered for HTML, Microsoft Word, OpenOffice, LibreOffice, EPUB, TeX, PDF, etc.

If all my blog post were written in Markdown, I could go back in time and offer a PDF/EPUB version of every blog post I ever did. Not as easy with HTML if things are not standardized. 

### Converting HTML to Markdown

I'm currently toying with [Hexo][7]. It has many converter including one that supports RSS. It managed to import all my blog post (tags included) but I was left with a bunch of Markdown files that needed some very tough love.

Just like any legacy code, I went through my legacy writing and removed all remaining HTML left from the conversion. Most of it was removed mind you. But the code one? They could not be converted properly. I had to manually remove `pre` and `code` tag every where. Indentation also was messed up from previous import. This had to be fixed. 

Right now, I regenerated a whole copy of my blog without breaking any article link. All the code has been standardized, indented and uses a plugin to display them properly. I change the theme or blog engine? I just take my MD files with me and I'm mostly good to go. 

### Deploying it to Azure

Once you have a working directory of [hexo][7] running, it will generate its content in a `public` folder.

Since we only want to deploy this, we will need to add a file name `.deployment` at the root of our repository.

It's content should be:

```ini
[config]
project = public
```

You can find more options about this on the Kudu project page about [Customizing Deployments][8]

### Issues left to resolve

Unless I'm moving to an engine like Jekyll or Octopress, most blog engines do not support Markdown files as blog input. We're still going to have to deal with converter for the time being. 

[1]:http://en.wikipedia.org/wiki/WYSIWYG
[2]:https://mindmup.github.io/bootstrap-wysiwyg/	
[3]:http://www.tinymce.com/
[4]:http://commonmark.org/
[5]:https://github.com/progit/progit
[6]:http://johnmacfarlane.net/pandoc/
[7]:http://hexo.io
[8]:https://github.com/projectkudu/kudu/wiki/Customizing-deployments
