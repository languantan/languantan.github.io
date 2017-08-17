---
layout: post
title:  "Unit Test Pitfalls"
date:   2017-08-18 00:02:40 +0800
categories: unit-testing, learning
---

# Copy-pasta '_eh hello, you think u pastamania ah?_'

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
  it 'should return first colour' do
    expect(colour_at(3)).to eq '' # hmm, what value ah?
  end
end
{% endhighlight %}

When writing the expected result, you might be assume code is right/lazy to count rainbow/not know the colours of the rainbow...(don't judge)
<br />
so you go ahead run the test and u get..

{% highlight shell %}
Failure/Error: expect(colour_at(3)).to eq ''
    expected ''
    got 'blue'
{% endhighlight %}

If you really are that lazy, and copy this into the code above.. well, your test will pass but we all know why it passed..