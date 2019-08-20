---
layout: post
title:  "Unit Testing Pitfalls"
date:   2017-08-18 00:02:40 +0800
categories: [unit-testing, learning]
excerpt_separator: <!--more-->
---
Simple function to get the nth colour of the rainbow.
{% highlight ruby %}
def colour_at(index)
  colours = ['red', 'orange', 'yellow', 'green', 'blue', 'indigo', 'violet']

  colours[index]
end
{% endhighlight %}

<!--more-->
A unit test for this can look like this

{% highlight ruby %}
describe '#colour_at' do
  it 'should return third colour of rainbow' do
    expect(colour_at(3)).to eq '' # hmm, what value ah?
  end
end
{% endhighlight %}

When writing the expected result, you might assume code is right/lazy to count rainbow/not know the colours of the rainbow...(don't judge)
<br />
so you go ahead run the test and u get..

{% highlight shell %}
Failure/Error: expect(colour_at(3)).to eq ''
    expected ''
    got 'green'
{% endhighlight %}

### #copypasta
{% highlight ruby %}
describe '#colour_at' do
  it 'should return third colour of rainbow' do
    expect(colour_at(3)).to eq 'green'
  end
end
{% endhighlight %}
{% highlight shell %}
1 example(s), 0 failures.
{% endhighlight %}

think about it. if not guilty, move on!
<hr>
