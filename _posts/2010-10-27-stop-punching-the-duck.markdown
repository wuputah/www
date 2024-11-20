---
title: Stop punching the duck
date: 2010-10-27
layout: single
---

So there you were, minding your own business, doing some behavior
driven development, thinking everything is great. You test your
controllers, you spec your models, and now the final, dreadful step:
actually firing up the browser. Expecting to see some terrible CSS
disasters, you get a nice error screen. "That's weird," you say, "my
cucumber tests say this should work!"

Alas, enter the duck punch. In your testing environment, rspec and
cucumber are loaded. And that means so is [this little duck punch][2].
Seems like you used `errors_on` instead of using `errors.on`. This
worked just fine in your tests but now that rspec is not loaded, the
feature explodes.

And there's no test you can write to check for this.

Your philosophical view of the world broken, you give up programming and
[move to a cave][3], never to be seen by civilization again.

So please, before you punch a duck, think of the caves. There's only so
many left.

[2]: http://github.com/dchelimsky/rspec-rails/blob/master/lib/spec/rails/extensions/active_record/base.rb#L28
[3]: http://en.wikipedia.org/wiki/Jainism
