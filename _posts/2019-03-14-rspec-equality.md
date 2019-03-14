---
layout: post
comments: true
title:  "RSpec & Equivalence Matchers"
image: ''
date:   2019-03-10 00:06:31
tags:
- rails
- rspec
- TDD
- test-driven-development
- BDD
description: 'Learning RSpec Equivalence matchers'
categories:
- Learn RSpec
---

<img src="{{ "/assets/img/rspec/rspec.png"}}" alt="">

## What is RSpec ?

RSpec is a testing tool written to test Ruby code. It can be used to test Ruby program as well as parts of a Ruby on Rails application. It is a <a href="https://blog.testlodge.com/what-is-bdd/"><code>behavior-driven development (BDD)</code></a> framework which is extensively used in the production applications. The beauty of RSpec is that it is written in a shared language, which improves communication between tech and non-tech teams and stakeholders. The simplicity in the RSpec syntax makes it one of the popular testing tools for Ruby applications.

## RSpec Syntax

Lets first look at a basic syntax of a sample testcase. 

{% highlight ruby %}
  describe 'sample testcase' do
    it 'checks if true is equal to true' do
      x = true
      expect(x).to eq(true)
    end
  end
{% endhighlight %}

<ul>
  <li>
    <code class="highlighter-rouge">describe</code> keyword is used to start the collection of test cases and it can be followed by a string or class. The string or class is followed by the block.
  </li>
  <li>
    <code class="highlighter-rouge">it</code> keyword is also followed by a string and the string is followed by the block.
  </li>
  <li>
     <code class="highlighter-rouge">expect(x).to eq(true)</code> is translated as the test case expects x to be equal to true.
  </li>
</ul>


## Equivalence Matchers

Today I will talk about the most commonly used RSpec Matcher known as Equivalence Matcher. My focus will be to highlight the significance of each of the provided Equivalence matchers by RSpec. So lets look into the its variants.

### Loose Equality (`eq`)

Loose equality matcher `eq` can be called on two objects where `almost equality is needed`.

{% highlight ruby %}
  a = "3 balls"
  b = "3 balls"
  expect(a).to eq(b) #Passes the test
{% endhighlight %}

However, there can be a case when we are given a result of decimal (with zero in decimal places) which is being compared with an integer. It will also pass with loose equality. Lets look at an example:

{% highlight ruby %}
  number = 17
  decimal = 17.00
  # 17 and 17.00 is different but "close enough"
  expect(number).to eq(decimal)      #Passes the test

  #Another way of checking loose equality
  expect(number).to be == decimal    #Passes the test
{% endhighlight %}

So `eq` is checking the objects loosely, not concerned with their specific types i.e. integer and decimal comparison can be passed through the testcase.
  

### Value Equality (`eql`)

Now, while writing a program there will be situations where you want to make sure decimal is not equal to an integer i.e. `you want the values to match exactly`.
  
{% highlight ruby %}
    number = 17
    decimal = 17.00
    # 17 and 17.00 are different but "close enough"
    expect(number).to eql(decimal)      #Fails the test
{% endhighlight %}

Now lets see what happens if we say 17 and 17.00 are not value equal. Lets replace `eq` by  `not_to eql`.

{% highlight ruby %}
    number = 17
    decimal = 17.00
    # 17 and 17.00 not the same, close doesn't count 
    expect(number).not_to eql(decimal) #Passes the test
{% endhighlight %}

### Identity Equality (`equal`)

Sometimes you want that the object should be same. This means that the two objects compared should be exactly same (having same memory allocation).
Behind the scenes of `equal`, Ruby checks if the object_id of both objects are same.


  
{% highlight ruby %}

    it 'will match identity equality with #equal' do
      a = "2 cats"
      b = "2 cats"
      expect(a).not_to equal(b) # same value, but different object

      c = b                     #c.object_id is same as b.object_id
      expect(b).to equal(c)     # same object
      expect(b).to be(c)        # synonym for #equal
    end
    
{% endhighlight %}



To sum up the discussion, you should use
<ul>
<li><code class="highlighter-rouge">eq</code> when we want to check if the values are almost equal.</li> 
<li><code class="highlighter-rouge">eql</code>  when we want to check if the values are exactly equal.</li> 
<li><code class="highlighter-rouge">equal</code>  when we want to check if the objects are same.</li> 
</ul>



