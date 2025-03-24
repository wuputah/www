---
title: "Snow Leopard: Upgrading for Rails Developers"
date: 2009-08-29
layout: single
---

Upgrading to Snow Leopard is not as easy as Apple would lead you to believe; at least not if you're a Rails developer. Here are a few select errors you might have encountered:

<pre>uninitialized constant MysqlCompat::MysqlRes
dlopen(/Library/Ruby/Gems/1.8/gems/nokogiri-1.3.3/lib/nokogiri/nokogiri.bundle, 9): no suitable image found</pre>

These errors seems to stem because OS X is now fully 64-bit, and unfortunately, all your compiled libraries are 32-bit. Whoops!

So, let's fix them.

**Important**: These instructions are tailored for Intel 64-bit machines, as only 64-bit machines should have these issues. Any Intel Core 2 machines including recent Macbooks, Macbook Pros, and iMacs, and all Mac Pros should work fine with these instructions.

### Install 64-bit MySQL

To fix the MySQL problem, you need install the <a href="http://dev.mysql.com/get/Downloads/MySQL-5.1/mysql-5.1.35-osx10.5-x86_64.dmg/from/pick#mirrors">x86_64 version of MySQL</a>. This will uninstall your old MySQL version, but will not migrate your databases. In order to migrate your data, first make sure MySQL is not running, then:

<pre>$ sudo mv /usr/local/mysql/data /usr/local/mysql/data.default
$ sudo mv /usr/local/mysql-oldversion/data /usr/local/mysql/data</pre>

Start up MySQL and your databases should be in tact.

### Reinstall the MySQL gem

Now that you have the correct version of MySQL, you need to reinstall the latest MySQL gem. Make sure to uninstall your current MySQL gem first:

<pre>$ sudo gem uninstall mysql
$ sudo env ARCHFLAGS="-arch x86_64" gem install mysql -- --with-mysql-config=/usr/local/mysql/bin/mysql_config</pre>

### Re-install MacPorts

To fix nokogiri (and other gems with external dependencies), you'll need to fix your Macports install. Unfortunately, the only way to fix Macports is to completely reinstall it. You need to completely delete (or at least move) your /opt/local directory. Once that is done, download and install the latest version of <a href="http://distfiles.macports.org/MacPorts/MacPorts-1.8.0-10.6-SnowLeopard.dmg">MacPorts for Snow Leopard</a>.

You'll probably want to install two things pretty quickly: libxml2 for nokogiri and git-core:

<pre>$ sudo port install libxml2
$ sudo port install git-core +bash_completion +doc</pre>

### Re-install nokogiri (and other gems)

Any gem that has a compiled component will need to be reinstalled. nokogiri is just one of the libraries; others include ruby-debug, ruby-prof, and a lot of others. To fix them, just run this command:

<pre>$ sudo gem pristine --all</pre>
