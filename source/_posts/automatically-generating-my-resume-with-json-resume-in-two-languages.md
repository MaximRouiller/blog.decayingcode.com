---
title : "Automatically generating my resume with JSON Resume in two languages"
date: 2017-05-03 13:00:00
tags: [nodejs, javascript, static content]
---
> **[TLDR: skip directly to the code](https://github.com/MaximRouiller/resume)**

I've often touted the advantages of statically generated websites. Security, ease of scaling, etc.

One the point I normally touch is that when you start dynamically generating websites, you often think of the data a different way. You see sources and projections. Everything is just waiting to be transformed into a better shared format.

Now comes the resume. If you guys haven't noticed yet, I'm a native French speaker from the Land of Poutine. You might think that we do everything in French but that is just wrong. Some clients will require English communication while others will require French. What that means is that when I submit a resume for a permanent job or a contractual work, I need to maintain two set of resume. One in each language.

While most people will only maintain a single Word document that will follow their graduation to their death, I have two. So every time I add something new to it, I have to reformat everything. It's a pain. More than maintaining just one. I was tired of always maintaining the same set of data so set myself to skip the *formatting* part of resume generation to a minimum.

### Introducting [JSON Resume][JsonResume]

That's where I found a JSON standard for resume called [JSON Resume][JsonResume]

An example of the schema can be found [here](https://jsonresume.org/schema/).

Here's what a very trimmed down version looks like:

```json
{
  "basics": {
    "name": "John Doe",
    "label": "Programmer",
    "picture": "",
    "email": "john@gmail.com",
    "phone": "(912) 555-4321",
    "website": "http://johndoe.com",
    "summary": "A summary of John Doe...",
    "location": {
      "address": "2712 Broadway St",
      "postalCode": "CA 94115",
      "city": "San Francisco",
      "countryCode": "US",
      "region": "California"
    }
  },
  "work": [{
    "company": "Company",
    "position": "President",
    "website": "http://company.com",
    "startDate": "2013-01-01",
    "endDate": "2014-01-01",
    "summary": "Description...",
    "highlights": [
      "Started the company"
    ]
  }],
  "education": [{
    "institution": "University",
    "area": "Software Development",
    "studyType": "Bachelor",
    "startDate": "2011-01-01",
    "endDate": "2013-01-01",
    "gpa": "4.0",
    "courses": [
      "DB1101 - Basic SQL"
    ]
  }],
  "skills": [{
    "name": "Web Development",
    "level": "Master",
    "keywords": ["HTML","CSS","Javascript"]
  }]
}
```

Ain't that awesome? You have something that is basically a representation of your experience, education and your personal information about you. You can even group your skills into nice little categories and set a proficiency level.

Now we need to generate something out of this otherwise, it's useless.

### Generating HTML from JSON Resume

**[TLDR: skip directly to the code](https://github.com/MaximRouiller/resume)**

I'll be frank, I tried the [CLI tool][JSONCLI] that came with the website. The problem is... it doesn't quite work. It works if everything is the default option. Otherwise? The tool looked broken. So I've done something different. I picked a theme, and copied its stylesheet and it's template into my own repository to create my own resume.

I don't even need to ensure backward compatibility. In fact, you could, easily, reverse engineer any of these theme and have something working within minutes.

#### My Theme: [StackOverflow](https://github.com/francescoes/jsonresume-theme-stackoverflow)

While the [index.js](https://github.com/francescoes/jsonresume-theme-stackoverflow/blob/master/index.js) might be interesting to see how they assemble together, it's just a simple [HandleBars](http://handlebarsjs.com/) file with some CSS. Nothing easier. Copying those three files will allow you to generate HTML out of it.

The package you might need in your own NodeJS app will be described [here](https://github.com/francescoes/jsonresume-theme-stackoverflow/blob/master/package.json). We're talking HandleBars and momentjs.

#### Re-importing everything.

The first thing I did, was create my own `index.js` to get cracking.

```Javascript
(function () {
  // this is a self executing function.
}());
```

With this, let's get cracking. First, we need some dependencies.

```Javascript
var fs = require("fs");
var path = require("path");
var Handlebars = require("handlebars");

// these are refactored HandleBars helpers that I extracted into a different file
var handlebarsHelpers = require("./template/HandlebarHelpers");
handlebarsHelpers.register();
```

Then you need to get your JSON Resume file that you painstakingly wrote.

```javascript
var base = require("./MaximeRouiller.base.json");
var english = require("./MaximeRouiller.en.json");
var french = require("./MaximeRouiller.fr.json");
```

I chose to go with a base that set the common values among both resume.

Next, let's make sure to create an output directory where we'll generate those nice resume.

```javascript
var compiled = __dirname + '/compiled';
if (!fs.existsSync(compiled)) {
  fs.mkdirSync(compiled, 0744);
}
```

Finally, we need a render function that we'll call with 2 different objects. One for French, the other one for English.

```javascript
function render(resume, filename) {
  // TODO next
}

render(Object.assign(base, english), 'MaximeRouiller.en');
render(Object.assign(base, french), 'MaximeRouiller.fr');
```

`Object.assign` is the fun part. This is where we merge the two JSON files together. This is a dumb merge so existing properties will get overwritten. That includes array.

So now that we have this, the `render` function actually need to generate the said HTML.

```javascript
function render(resume, filename) {
    // reading the file synchronously
    var css = fs.readFileSync(__dirname + "/template/style.css", "utf-8");
    var tpl = fs.readFileSync(__dirname + "/template/resume.hbs", "utf-8");

    //Write to specified filenamem, the generated HTML from Handlebars.compile
    fs.writeFile(compiled + "/" + filename + ".html", Handlebars.compile(tpl)({
      css: css,
      resume: resume
    }), {
      flag: 'w'
    });
}
```
That's it for HTML. `Handlebars.compile(tpl)` will create an executable function that needs parameters to execute. First, the CSS itself and finally, the resume object that contains the model from which we generate everything.

The write flag is just to ensure that we overwrite existing files.

Running `node index.js` will now generate two files in `/compiled/`. `MaximeRouiller.en.html` and `MaximeRouiller.fr.html`.

HTML is great but... most recruiting firms will mostly only work with PDF.

### Modifying our script to generate PDF in the same folder

First, we need to install a dependency on `phantomjs-prebuilt` with `node install phantomjs-prebuilt --save`.

Then we require it: `var phantomjs = require('phantomjs-prebuilt');`

Then you download the [rasterize.js](https://github.com/ariya/phantomjs/blob/master/examples/rasterize.js) file and you copy this code right after we generate our HTML.

```javascript
var program = phantomjs.exec('./scripts/rasterize.js', './compiled/' + filename + '.html', './compiled/' + filename + '.pdf', 'Letter')
program.stdout.pipe(process.stdout);
program.stderr.pipe(process.stderr);
```

Same comment except we're piping the HTML we just generated into PhantomJS to generate HTML with the `Letter` US format of 8 and a half inches by 11 inches.

That's it.

### The possibilities

In my scenario, I didn't require any other format. I could have generated more with something like [pandoc](http://pandoc.org/) and it would be a good way to generate a thousand different version of the same resume in different format.

When creating new ways to make the same old, I like to think of the different possibilities this could get used in.

* Storing a database of resume and generating them on the fly in a company's format in a minute.
* Changing your resume in 25 different themes easily
* Instead of using Handlebar to generate in node, we could have generate the same resume with a barebone ASP.NET Core with Razor using my [Poorman static site generator](/post/using-static-content-generation-in-asp.net-core/). This could allow you to localize your template easily too or using a familiar content rendering.

In any case, my scenario was easy and there's no way I'm pushing farther than is required.

Do you see other possibilities I missed?

[JsonResume]: https://jsonresume.org/
