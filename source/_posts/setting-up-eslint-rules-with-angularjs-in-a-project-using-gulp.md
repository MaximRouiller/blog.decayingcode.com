---
title : "Setting up ESLint rules with AngularJS in a project using gulp"
date: 2017-02-08 10:00
tags: [angularjs, javascript, gulp]
---

When creating Single Page Application, it's important to keep code quality and consistency at a very high level.

As more and more developer work on your code base, it may seem like everyone is using a different coding style. In C#, at most, it's bothersome. In JavaScript? In can be down right dangerous.

When I work with JavaScript projects, I always end up recommending using a linter. This will allow the team to make decisions about coding practices as early as possible and keep everyone sane in the long run.

If you don't know [ESLint](http://eslint.org) yet, you should. It's one of the best JavaScript linter available at the moment.

### Installing ESLint

If your project is already using gulp to automate the different work you have to do, `eslint` will be easy to setup.

Just run the following command to install all the necessary bits to make it run-able as a gulp task.

```bash
npm install eslint eslint-plugin-angular gulp-eslint
```

Alternatively, you could also just install `eslint` globally to make it available from the command line.

```bash
npm install -g eslint
```

### Configuring ESLint
Next step is to create a `.eslintrc.json` at the root of your project.

Here's the one that I use.

```json
{
  "env": {
    "browser": true
  },
  "globals":{
      "Insert Global Here": true
  },
  "extends": "angular",
  "rules": {
    "linebreak-style": [
      "error",
      "windows"
    ],
    "semi": [
      "error",
      "always"
    ],
    "no-console": "off",
    "no-debugger": "warn",
    "angular/di": [ "error", "$inject" ]
  }
}
```

First, setting up the environment. Setting `browser` to true will import a tons of globals (`window`, `document`, etc.) that tells ESLint that the code is running inside a browser and not in a `nodejs` process for instance.

Next are globals. If you are using libraries that defines globals and that you are using those globals, it's where you would define them. (Eg.: `jQuery`, `$`, `_`)

`extends` will allow you to define the base rules that we will follow. `angular` basically enables the plugin we have as well as all the basic JavaScript rules defined by default.

`rules` will allow you to customize the rules to your liking. Personally, I don't like seeing the `console` and `debugger` errors so I adjusted them the way I like. As for `angular/di`, it allows you to set your prefered way of doing dependency injection with Angular. Anything that is not `service.$inject = [...]` will get rejected in my code base.

### Sub-Folder Configuration

Remember that you can always add rules for specific folders. As an example, I often have a `service` folder. This folder only contains services but the rule `angular/no-service-method` will have an error for each of them.

Creating an `.eslintrc.json` file in that folder with the following content will prevent that error from ever showing up again.

```json
{
  "rules": {
    "angular/no-service-method": "off"
  }
}
```

### Creating a gulp task

The gulp task itself is very simple to create. The only thing that is left to pick, is the format in which to display the errors.

You can pick from [many formatters](http://eslint.org/docs/user-guide/formatters/) that are available.

```javascript
var eslint = require("gulp-eslint");
gulp.task('eslint', function () {
    return gulp.src(src + '**/*.js')
        .pipe(eslint())
        .pipe(eslint.format('stylish'));
});
```

### Customizing your rules

As with every team I collaborate with, I recommend that everyone sits down and define their coding practices so that they may be no surprise.

Your first visit is to check the [available rules](http://eslint.org/docs/rules/) on ESLint website. Everything with a checkmark is enabled by default and will be considered an error.

I wouldn't take too much time going through the list. I would, however, run eslint on your existing code base and see what your team consider errors and identify cases where something should be errors.

* Does extra-parenthesis gets on everyone's nerves? Check out [no-extra-parens](http://eslint.org/docs/rules/no-extra-parens)
* Does empty functions plaguing your code base? Check out [no-empty-function](http://eslint.org/docs/rules/no-empty-function)
* Do you consider using `eval()` a bad practice (you should!)? Add [no-eval](http://eslint.org/docs/rules/no-eval) to the rulebook!

### Improving your code one step at a time

By implementing a simple linter like ESLint, it's possible to increase your code quality one step at a time. With the angular plugin for eslint, it's also now possible to improve your Angular quality at the same time.

Any rules you think should always be enabled? What practices do you use to keep your code base clean and bug free? Let me know in the comments!
