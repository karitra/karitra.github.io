---
layout: post
title:  "Generate placeholders sequence in C++"
date:   2017-06-13 18:58:48 +0300
categories: c++ templates 
---

*WIP*

While mocking with cocaine-framework (native), precisely with service methods implementaion, I have came up to the code repetition of placeholders _placement_ with `std::bind`:

{% highlight cpp %}
using ph = std::placeholder;

on<event1>(std::bind(foo, ph::_1, ph::_2));
on<event2>(std::bind(bar, ph::_1, ph::_2, ph::_3));
on<eventN>(std::bind(zoo, ph::_1, ph::_2, ph::_3, ph::_4, ph::_5));
{% endhighlight %}

While I found it quite a common pattern within C++ frameworks trying to implement callbacks support with `std::bind`, the practice with sequental `ph::_N` seems redundant and annoying, so after some googling and studying SO, following solution for placeholders auto generation was formed. 

In cocaine-framework the service API is staticaly defined at compile time via `boost::mpl` containers, so the number of methods arguments is also known at compile time, in the sample above `eventN` actually a struct tag describing a method arguments:

{% highlight cpp %}
struct event1 {
    using arguments_type = boost::mpl::list<
        int,
        double
    >;
};
{% endhighlight %}

so to the arguments count is just a:

{% highlight cpp %}
boost::mpl::size<event1::arguments_type>::value;
{% endhighlight %}

The idea is to combine integral sequence generator and custom placholder. 

Complete sample [here][complete-sample]

[complete-sample]: https://github.com/karitra/cpp-playground/blob/master/src/ph.cc

