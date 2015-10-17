---
title: "My Awesome Book"
permalink: chapter/1.html
layout: chapter
---
<a name="{{ page.permalink }}"></a> <!-- Leave this here -->

This is my awesome book!

And this is the first chapter!

### Generators
If you have Ruby installed, you can use the chapter generator. Just run `rake generate:chapter` at the command line to generate a new chapter. You can provide an argument to the rake task to include a title, like so: `rake generate:chapter['My Awesome Title']`.


### _posts directory
Like a typical Jekyll site, dynamic content (in this case, our chapters) are in the `_posts` directory. Jekyll posts need a date in the file name, hence the `1900-01-01` thing in the file names.
