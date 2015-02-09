---
layout:    post
category:  ruby
tags:      ruby sinatra rspec helpers error nomethod
title:     Testing sinatra helpers with rspec
---
Lately I built a simple [Sinatra][1] based demo application ([YaSApp][2]) and tried to test some methods within a **helpers block**

{% highlight ruby %}
helpers do
  def human_date(datetime)
    datetime.strftime('%-d. %B %Y, %H:%M Uhr')
  end
end
{% endhighlight %}

But [**Rspec**][3] kept me sending **NoMethod Errors**:

    NoMethodError:
      undefined method `human_date' for #<Sinatra::Application ...

Than I learned in the context of the [Padrino][4] framework, that this is a "special" **Sinatra** feature - so called "shorthand helpers". Because above statement is shorthand for:

{% highlight ruby %}
helpers = Module.new do
  def human_date(...
  end
end
{% endhighlight %}

Which is kind of a anonymous module and that way hard to reference or test!

The solution is to make a explicit module and propagate it to the application like this:

{% highlight ruby %}
module AppHelpers
  def human_date(datetime)
    datetime.strftime('%-d. %B %Y, %H:%M Uhr')
  end
end

Sinatra::Application.helpers AppHelpers
{% endhighlight %}

and then it is testable in **RSpec**:

{% highlight ruby %}
describe "AppHelpers" do
  subject do
    Class.new { include AppHelpers }
  end

  describe "#human" do
    it "formats date and time readable" do
      t = Time.new(2014,02,06,14,18,33)
      expect(subject.new.human_date t).to eq '6. February 2014, 14:18 Uhr'
    end
  end
end
{% endhighlight %}

Hope thats helpful for one or the other?

[1]: http://www.sinatrarb.com/
[2]: https://github.com/netzfisch/yasapp
[3]: https://relishapp.com/rspec/
[4]: https://github.com/padrino/padrino-framework/issues/930
