---
title: Hello blog! 🚀
author: Louis D'hauwe
category: post
tags: blog, markdown, swift
excerpt: This is the first post of the Silver Fox blog, a place where I will share interesting findings, new projects and just random thoughts.
---
<p>
This is the first post of the Silver Fox blog, a place where I will share interesting findings, new projects and just random thoughts. Kinda like <a href="https://twitter.com/LouisDhauwe">my Twitter feed</a>, but without a character limit.
</p>

## Introduction
My name is Louis D'hauwe, I'm a Belgian programmer born in the late nineties. I currently work as a full time iOS developer at [Next Apps](http://www.nextapps.be). My full time job plus my side projects can easily add up to about 50 to 60 hours a week of writing or reading code. As of late, most of this code is Swift (though not all!). It'll be of no suprise that most posts will be about some code.

## Starting a blog in 2017
For a couple of years now I've been thinking about starting a blog. Starting one is pretty easy nowadays. [Medium](https://medium.com), [Squarespace](https://www.squarespace.com) and others can get you started in a matter of minutes. However, for my blog I wanted a little more control.

### Static content
I decided that all I really need is static webpages. I don't need a comment section, or some admin panel to write posts. But obviously writing blogposts in HTML directly is tedious, there's need for an abstraction. The ideal candidate (from a software developer's perspective) is Markdown.

Each post (including this one) is written in a Markdown file. These are converted to static HTML pages by a Swift command line tool that I wrote myself. 

### Open source
I have open sourced four projects that together make this whole site possible:

* [silverfox-site](https://github.com/louisdh/silverfox-site) – the HTML, CSS & JavaScript
* [silverfox-articles](https://github.com/louisdh/silverfox-articles) – the blog posts (.md files)
* [silverfox-news](https://github.com/louisdh/silverfox-news) – news items about my projects (.md files)
* [silverfox-gen](https://github.com/louisdh/silverfox-gen) – generator for the static content

### silverfox-gen
This is perhaps the most interesting of the four. This is a Swift command line tool that takes its input from silverfox-articles and silverfox-news. It outputs the result to silverfox-site.

There are five main aspects to silverfox-gen:
#### 1. Files
Since this takes a bunch of files as input and needs to output generated files, easy file management is essential. To handle this, I used [John Sundell](https://twitter.com/johnsundell)'s amazing [Files](https://github.com/JohnSundell/Files) framework.

#### 2. Markdown support
To convert the Markdown to HTML, I decided to use [Markdown by Vapor Community](https://github.com/vapor-community/markdown). This is a Swift wrapper around [cmark](https://github.com/github/cmark), the Markdown parser developed and used by GitHub.

#### 3. Syntax highlighting
Posts will often include code. But cmark doesn't add syntax highlighting, it only translates the Markdown to HTML. To add syntax highlighting to the static pages, I used [Prism.js](http://prismjs.com). In combination with Apple's [JavaScriptCore](https://developer.apple.com/reference/javascriptcore) framework, this enables a nice String extension in Swift to generate HTML with classes for each token to be highlighted. All that's left after that is to add some CSS.

#### 4. Dependency manager 
silverfox-gen was developed to run on macOS. This gave me the opportunity to use the [Swift Package Manager](https://swift.org/package-manager/). The biggest advantage of this is that it's included in Xcode. So there's no need to install a command line tool.

#### 5. RSS 
This blog supports RSS, which requires an xml file to be generated with all the posts. Luckily, Foundation on macOS includes XMLDocument. Using this, I was able to write an RSS feed generator in about 30 minutes. 

An excerpt of the generation code using the Files framework:

```swift
let rssFile = try outputRoot.file(atPath: "silverfox-rss.xml")

try? rssFile.delete()

let rssString = generateRSS(for: articles)

try rssFile.write(string: rssString)
```


## Getting notified
If you would like to get notified about new posts, you can:

* Follow [@SilverFoxBE](https://twitter.com/SilverFoxBE) on Twitter
* Subscribe to the [RSS feed](http://silverfox.be/silverfox-rss.xml)