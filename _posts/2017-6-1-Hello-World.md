---
layout: post
title: (First) Lesson!
---

Welcome!
Just started this blog out of pure boredom and decided I needed to be a bit productive with myself.
I am currently an aspiring Software Engineer, just finished university at UC Irvine, and headed towards the dreaded SF App Academy this July.
I will use this canvas to write about my personal life and explanations about things I learn.
Well if anyone is listening,
See you soon!

{% highlight ruby linenos %}
def show
  puts "Outputting a very lo-o-o-o-o-o-o-o-o-o-o-o-o-o-o-o-ong lo-o-o-o-o-o-o-o-o-o-o-o-o-o-o-o-ong line"
  @widget = Widget(params[:id])
  respond_to do |format|
    format.html # show.html.erb
    format.json { render json: @widget }
  end
end
{% endhighlight %}
