---
category: article
date: 2017-12-12 06:00:00 +1100
description: A handful of approaches to create and retain quality CSS
draft: false
header-image: stop-writing-crappy-css-header.jpg
layout: article.html
tags: css, sass, less, post css, bootstrap css, foundation css
thumb: stop-writing-crappy-css.png
title: Stop writing crappy CSS
---
## Ever written crappy CSS?
You set out to write a masterpiece but now your code looks like something you reheated after a long night of drinking. It happens to the best of us. It's OK, we've all written CSS we are not proud of. 

Sure, you're using [SASS](http://sass-lang.com/), [LESS](http://lesscss.org/) or even [postcss](http://postcss.org/) but it's not helping you to remember why you wrote those styles six months ago and somehow you are compiling far more CSS than you would expect. In this article I'm going to address some typical problems developers have when writing CSS and (hopefully) provide some advice to improve what you write.

### First things first, set a standard
This is generally good practice in any software language. If everyone on the project agrees on the approach then they are all going to write consistent CSS. This is going to lead to less duplicated code and make life easier for new developers. Choose a naming convention and folder structure, make sure it's documented correctly in your readme and ensure it is enforced via code review.

Here is a great article on [css methodologies](https://codepen.io/hidanielle/post/css-methodologies-naming-conventions-and-file-structures) if you are interested in some of the different approaches you may way to use.

### Build modular
Web development has come a long way and thankfully modular and component-based architecture have become an industry standard. Where component-based architecture aids CSS greatly is by showing a relationship between your markup and your styling. Writing your CSS in isolation to its associated HTML and JavaScript makes removing functionality difficult and can result in redundant CSS being left in the build. 

If your application is using [Angular](https://angularjs.org/) or [React](https://reactjs.org/) their conventions force you to take this approach. If you are not currently building modular I highly encourage you to start doing so.

### Be disciplined with source control
If you currently don't have a code review process in place you should start. It doesn't matter if you are the only developer on the project or the most senior person on the team. If you are a lead don't be afraid to ask junior staff to review your code. If you are flying solo then create a pull request for yourself and review it with fresh eyes the following day.

### Weigh up the pros and cons of using a framework or grid system
There's at least two benefits in using frameworks like [Bootstrap](https://getbootstrap.com/) or [Foundation](https://foundation.zurb.com/) that I think help prevent new developers making a mess in your build.
  1. Having an established grid means new developers can convert designs into markup without having to understand bespoke code. It also lowers the chance they will rewrite existing styles.
  2. Some frameworks have their own supported libraries for components. This means as new features are added to the build you can simply take the parts of the framework you need off the shelf without having to either go fish for a third-party library that fits your needs or writing more bespoke code. This results in your codebase being cleaner, saves developers on your team time and in the case of using third party tools midigates the risk of being let down by bugs in the library after becoming wed to it.

Of course, my little caveat to all this is to weigh up the advantages us using a framework. You are responsible for making decisions about what goes into your code base. If you are adding libraries that add more file size than they do value you should be exploring other options.

There's a great article by Timothy Marks [Evaluating CSS frameworks](https://codeburst.io/evaluating-css-frameworks-bulma-vs-foundation-vs-milligram-vs-pure-vs-semantic-vs-uikit-503883bd25a3) which I would recommend if you are considering using a framework and need a breakdown of some of the options that are out there.

### Don't be precious
[YAGNI](https://en.wikipedia.org/wiki/You_aren%27t_gonna_need_it), learn it and live by it. Every time you commit a block of commented out code I die a little inside. Yes, your pre-compiler will remove this stuff from production but it just makes your CSS harder to read an potentially confusing. Don't do it. 

This is even more important for code that you just really want to keep for no justifiable reason. If it's not used get rid of it.

### Readme like a boss
If new developers are having trouble gaining velocity on your project the chances are it's your fault. The readme file of your project should **have everything a new developer should know in it**. This includes,
  * Project set up.
  * Process for committing code.
  * Coding standards and examples.
  * Environments.

### Don't over complicate things
Another typical hole developers fall into is attempting to write code that's going to accommodate every scenario possible. It's great when you have something that is flexible enough to work in different circumstances but the chances are that what you write is only going to cater for a single purpose. This tends to usually mean that the CSS you write will be too complicated for other developers to understand and may cause them to try and write around it. Not to mention you are adding to your builds file size without needing to. Over-engineering a feature also results in pouring more hours into a project than required or estimated.

### Add linting to your build process
Linting may seem like an unnecessary hassle to some developers but I can assure you it's a great way to keep your team honest. Not only does it help enforce the coding standard you have set for the project but it's going to save you time when doing code reviews as the feature branch will have already been checked for syntax and styling errors.

As an aside you could use [githooks](https://git-scm.com/docs/githooks) to run your lint prior to committing code so stop developers from committing anything that doesn't abide by your coding standards.

### Know when it's time to refactor
As a developer it is part your job to decide when sections of CSS are having an impact on the productivity of the team and bring it to the attention of the product owner. How ever try to be as objective as possible, we developers are quick to want to refactor. Especially when inheriting legacy code.

### So what did we learn?
  * Document a coding standard to keep your CSS consistent
  * Keep your documentation up to date to help future developers.
  * Decide on a folder structure that shows a relationship between your CSS and HTML/JS.
  * Clean up after yourself. Don't leave files of blocks of code in the build because you think
  they will be relivant eventually.
  * Decide if a CSS framework is actually going to improve the quality and productivity of your project.
  * Code reviews to (hopefully) stop crappy CSS sneaking into the build.
  * Lint.
