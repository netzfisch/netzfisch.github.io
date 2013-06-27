---
layout: post
category: ruby
tags: coding rspec ruby
title: Testing vs. stubbing pagination of will_paginate gem
---

I used a "rspec view" as customer acceptance test. My intention was not to test the "will_paginate" gem code, but to assure the customer, that there is a "pagination bar"! 

At the beginning I tried to stub the pagination method like that

{% gist 5605721 %}

which was not working and did not make sense in a view spec (vs. a model spec). Thanks to hints from [David](1): 

_"First, yours stubs :will\_paginate. You definitely don't want to do that if your specifying outcomes of will\_paginate."_

and [Mislav](2):

_"The way to get pagination in the view is to create a fake paginated collection that appears to have more than 1 page. You only have 3 records, and with the default per\_page value this is only 1 page of data."_

I changed the spec to

{% gist 5605792 %}

which did the trick and worked perfectly.

Find here the full [rspec](1) and [will_paginate](2) threads! Next time I probably would better use rspec features for that!

[1]: https://groups.google.com/forum/#!topic/rspec/sRqx0rXzFKQ "rspec"
[2]: https://groups.google.com/forum/#!topic/will_paginate/i2LCboRZWfs "will_paginate"
