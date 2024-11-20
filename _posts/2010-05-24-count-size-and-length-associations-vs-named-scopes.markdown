---
title: "count, size, and length: associations vs named scopes"
date: 2010-05-24
---

I was looking into this today and it was a bit different--and much more complex--than what I expected, so I figured I'd pass on the results.~

On an association:

* `length` forces the association to be loaded if it is not already, then checks the size of the array.

* `count` always invokes a `COUNT()` query, regardless of the state of the association (loaded or not). It does not take unsaved records into account, and does not respect active scopes (which is why named scope does not use this). The count is never cached by Rails, but would be cached by the Rails query cache.

* `size`'s main difference is that it includes unsaved records in its result, but it can only do so if knows about them. Associations only know about records that are queued for saving when the parent object is saved, and it can only do so if called with `association.<<` or `association.build`. (These are the same cases as when a `counter_cache` column is automatically updated; <em>caveat emptor</em>.) `size` can be more efficient than `length` in some cases - for `has_many` and `has_many/has_one through` associations, it takes advantage of `counter_cache` columns or invokes a `COUNT()` query. There is no optimization for HABTM associations.

This gets a bit trickier when you add named scopes into the mix: the results change because named scope has taken over some of the methods. To understand this, you must remember that calling a named scope creates a new `Scope` object which wraps the upstream object. In addition to implementing the required querying magic, it provides a separate layer of caching entirely separate from the upstream object.

* `length` is not implemented by named scope, and so falls back to either the association's implementation, which will work because the eventual `find(:all)` will be scoped, or `Array#length` if there is no association defined.

* `count` is not implemented by named scope, but does not send this method to the association's implementation, as the result would be incorrect. It always invokes an SQL count by using `with_scope()` and invoking the original count method on the class (defined by `ActiveRecord::Calculations`) with the appropriate parameters. This is probably the most confusing of the 6 cases.

* `size` is implemented by named scopes, and either returns the length of the loaded array, or invokes an SQL count if it hasn't been loaded in all cases. The association behavior of handling unsaved records is gone as well as any association-specific optimizations (or lack thereof).

Keep in mind that both object associations and named scopes can only cache objects internally if they're the same instance (i.e. saved to a variable), so two otherwise identical named scopes/association calls will only be cached at the query cache level (whether by Rails or the database). `self` often serves as this instance for an association when writing model code, but once you use a named scope, these are much more rarely cached, so `count` is often the best choice if a named scope is involved. Otherwise, `size` or `length` may be faster, depending on the scenario, but it still requires a full association load before it can cache the size. However, if you're using `counter_cache`, you must use `size` to take advantage of them (unless you read the counter attribute directly).

Lastly, while `count` is a good choice, you shouldn't get in a habit of using it on everything. `Array#count` was added in ruby 1.8.7 (and later). `String#count` exists, but it takes a parameter and returns the count of the number of occurrences of characters (`'confused yet?'.count('ce')` returns `3`).
