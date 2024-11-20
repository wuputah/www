---
title: ERD diagrams from Sequel Pro
date: 2010-01-07
layout: single
---

If you need a diagram of your MySQL database and you're on a Mac, generating an ERD diagram is quite easy - and completely free. Sequel Pro can export Graphviz dot files, and then all you need is a few tools to create the diagram.

* Install graphviz from MacPorts via your Terminal:<br />`sudo port install graphviz`
* Install [Sequel Pro](http://www.sequelpro.com/), run the app, connect to your MySQL server and open the database you'd like to diagram.
* Go to File > Export > Graphviz Dot file, and save the file somewhere convenient.
* Generate an SVG file of your diagram:<br />`dot -Tsvg your_database.dot > your_database.svg`
* You can open SVG files with Opera, Safari, Illustrator, etc, but you can generate a PNG file in a number of ways. You can try installing ImageMagick or libsrvg from MacPorts, or use Illustrator or Inkscape to open and convert the file.
** ImageMagick: `convert your_database.svg your_database.png`
** libsrvg: `cat your_database.svg | rsvg-convert -o your_database.png`

The result is a basic table-based ERD, but it's not bad for a few minutes of your time.

Thanks to the [geert](http://github.com/finalist/geert) README for the introduction to this process.
