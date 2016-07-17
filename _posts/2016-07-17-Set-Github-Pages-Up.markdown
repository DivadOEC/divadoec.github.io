---
layout: post
title:  "Set Github Pages Up!"
date:   2016-07-17 21:01:40 +0800
categories: jekyll update
---

# Taobao's mirror
[https://ruby.taobao.org]
[https://npm.taobao.org/]

# Jekyll's Docs
https://jekyllrb.com/docs/home/

# Set Github Pages up step by step
{% highlight bash %}
sudo apt install ruby
// modify gem sources to taobao's mirror,see blow
sudo gem install bundler

// add a gemfile
echo "source 'https://ruby.taobao.org'" >Gemfile
echo "gem 'github-pages', group: :jekyll_plugins" >>Gemfile

// no sudo
bundle install

// Generate Jekyll site files
bundle exec jekyll new . --force

// jekyll serve
bundle exec jekyll serve

{% endhighlight %}

# gem sources
{% highlight bash %}
gem sources
gem sources -a https://ruby.taobao.org
gem sources -r https://rubygems.org/
{% endhighlight %}

# U Got *"Failed to build gem native extension"*,when you *"bundle install"*

1) /usr/bin/ruby2.0 extconf.rb mkmf.rb can’t find header files for ruby at /usr/lib/ruby/include/ruby.h
2) fatal error: zlib.h: No such file or directory

{% highlight bash %}
sudo apt install ruby-dev
// or
sudo apt install ruby2.3-dev

sudo apt-get install zlib1g-dev
{% endhighlight %}

# U Got *"Could not find a JavaScript runtime"*,when you *"jekyll serve"*

add these dependency to Gemfile
{% highlight bash %}
gem 'therubyracer'
gem 'execjs'
{% endhighlight %}

>
原因很简单，execjs根据不同平台依赖不同的js runtime：
therubyracer - Google V8 embedded within Ruby
therubyrhino - Mozilla Rhino embedded within JRuby
Johnson - Mozilla SpiderMonkey embedded within Ruby
Mustang - Mustang V8 embedded within Ruby
Node.js
Apple JavaScriptCore - Included with Mac OS X
Mozilla SpiderMonkey
Microsoft Windows Script Host (JScript)
选择任何一个js runtime是开发者自己的事情，如果Rails默认在Gemfile里加上therubyracer的话也会有很多人抱怨的。

>
https://github.com/rails/rails/pull/3619 这里已经有pull request可以在找不到系统里的js runtime就在gemfile里添加therubyracer的特性。但是木有被采纳。

