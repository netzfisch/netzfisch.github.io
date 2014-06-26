---
layout:    post
category:  ruby
tags:      coding authorisation ruby rails pundit rspec
title:     Pundit rails authorisation with RSpec
---
**Authorisation** is always an **issue** and have to be dealt carefully. Often
you start with simple **home made** statements like `user.admin?`, etc. For the
[TEAM ORGA][1] application also came the time to switch from a custom solution
to a **solid community proofed and backed solution**. First you will think about
the well known [CanCan][2] gem from Ryan Bates.

But I decided to use [Pundit][3] from the guys of [Elabs][4]. Mainly because of
the following reasons I decided to do so:

* Pundit uses "**regular Ruby** classes and **object oriented** design patterns
  to build a **simple**, robust and **scaleable** authorization system", while
  CanCan uses a pseudo DSL with a central `ability.rb` model.
* Though according to [The Ruby Toolbox][5] CanCan is much wider spread, it
  doesn't work out of the box with **Rails 4** and has - with few commits and
  many open issues - the **signs** of an **abandoned project**.

Thanks to the Pundit [readme][6] and two excellent blog posts

* [Migrating to Pundit from CanCan][7]
* [Testing Pundit Policies with RSpec][8]

the implementation was straight forward. If you use the alternative approach to
create Pundit policy specs as outlined in the posts - by scoping to a user
context, be aware that you need

* to enable a **custom matcher** as described in that [section][9] and
* avoid **namespace conflicts**, e.g. by choosing a different matcher name or
  **avoid** `require pundit/rspec` in `spec_helper.rb` !

Furthermore I found a **lack of documentation** concerning a RSpec examples for
`pundit_scope`, but that can be easily tested like this:

{% gist netzfisch/acc249f828884c739848 %}

[1]: https://github.com/netzfisch/teamorga/
[2]: https://github.com/ryanb/cancan/
[3]: https://github.com/elabs/pundit
[4]: http://www.elabs.se/
[5]: https://www.ruby-toolbox.com/categories/rails_authorization/
[6]: https://github.com/elabs/pundit/blob/master/README.md
[7]: http://blog.carbonfive.com/2013/10/21/migrating-to-pundit-from-cancan/
[8]: http://thunderboltlabs.com/blog/2013/03/27/testing-pundit-policies-with-rspec/
[9]: http://thunderboltlabs.com/blog/2013/03/27/testing-pundit-policies-with-rspec#a-custom-matcher-for-pundit
