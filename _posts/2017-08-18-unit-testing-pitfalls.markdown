---
layout: post
title:  "Unit Testing Pitfalls"
date:   2017-08-18 00:02:40 +0800
categories: unit-testing learning
---

### Copy-pasta '_eh hello, you think u pastamania ah?_'

{% highlight ruby %}
def colour_at(index)
  colours = ['red', 'orange', 'yellow', 'green', 'blue', 'indigo', 'violet']

  colours[index+1] # super contrived example to show bad logic
end
{% endhighlight %}

<br />

Simple function to get the nth colour of the rainbow. A unit test for this can look like this

{% highlight ruby %}
describe '#colour_at' do
  it 'should return third colour' do
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
    got 'blue'
{% endhighlight %}

### #copypasta
{% highlight ruby %}
describe '#colour_at' do
  it 'should return third colour' do
    expect(colour_at(3)).to eq 'blue'
  end
end
{% endhighlight %}
{% highlight shell %}
1 example(s), 0 failures.
{% endhighlight %}

think about it. if not guilty, move on!
<hr>
