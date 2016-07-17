---
layout: post
title:  "Set Sublime Text Up!"
date:   2016-07-17 21:01:40 +0800
categories: jekyll update
tags:   [Sublime Text,Package Control,Editor]
---

# Install Package Control
Use *Ctrl + `* to open the command windows of sublime,then enter these code and press *Enter*

## for ST3
{% highlight python %}
import urllib.request,os,hashlib; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://dn-52cik.qbox.me/' + pf.replace(' ', '%20')).read(); open(os.path.join( ipp, pf), 'wb' ).write(by)
{% endhighlight %}

## for ST2
{% highlight python %}
import urllib2,os,hashlib; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); os.makedirs( ipp ) if not os.path.exists(ipp) else None; urllib2.install_opener( urllib2.build_opener( urllib2.ProxyHandler()) ); by = urllib2.urlopen( 'http://dn-52cik.qbox.me/' + pf.replace(' ', '%20')).read(); open( os.path.join( ipp, pf), 'wb' ).write(by); print('Please restart Sublime Text to finish installation')
{% endhighlight %}

*Restart your ST*

# Modify Package Control Channel
1) Use *Ctrl + Shift + p* to open the command panel.
2) input *pcac* for add channel
3) Enter the following mirror
	>https://dn-52cik.qbox.me/channel_v3.json
	这个镜像只是个 json 文件，没有做真正的安装包镜像，首先空间不够，其次流量不够。。目前就提供了列表镜像，而列表里的安装包是 github 里的，基本上可以正常安装。
4) also, you can try the *parc*
