---
title: Moving attachment_fu from filesystem to S3 storage
date: 2010-06-27
layout: single
---

Most of this transition is fairly easy, but many users will find a small hiccup when it comes time to upload your files to S3.

1. Create a `config/amazon_s3.yml` file with your credentials.
2. Setup your S3 bucket, upload the files (I use [S3Fox](http://www.s3fox.net/)). Make sure to set the access to world-read for your uploaded files.
3. Add the `aws-s3` gem to your environment as well as your hosting platform or server.

Ah, but there is a small problem. How do you get from `attachment_fu`'s default of `0010/5198/filename.png` to `105198/filename.png`? The solution is a small Bash script. This is designed to be run from within the top-level folder for each of your `attachment_fu` directories, like `public/pictures`, `public/avatars`, etc.

<script src="http://gist.github.com/455215.js?file=rename-files-for-s3.sh"></script>

I'd recommend running this on a copy of your files, just in case I messed up, but the most important line is `let "d=$d1$d2"`. This wonderful line combines the `0010` and `5198` and treats them as a number, for a result of `105198`. I don't know much about `let`, but it seems to work wonderfully when you need to treat strings as integers.

PS. Don't forget step 3.
