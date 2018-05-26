---
layout: post
title: Webscraping Homework
image: /img/hello_world.jpeg
---

The task was to download all issues of "Richmond Times Dispatch" with wget. <!--more--> First I copied the source code of the [collection](http://www.perseus.tufts.edu/hopper/collection?collection=Perseus:collection:RichTimes/) into Sublime Text and searched with regular
expression `text[?]doc.+.2006.\d\d.\d\d\d\d` for all the links.

Since the links were not absolute link paths, I had to add `www.perseus.tufts.edu/hopper/dl` in front of the links using `^` and replacing. I saved them into a text file and downloaded the issues through the command line with the command: `.\wget.exe -i links.txt` 
