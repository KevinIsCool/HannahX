---
title: tags
layout: page
---

<div id='tag_cloud'>
{% for tag in site.tags %}
<a href="#{{ tag[0] }}" title="{{ tag[0] }}" rel="{{ tag[1].size }}">{{ tag[0] }}</a>
{% endfor %}
</div>

<ul class="listing">
{% for tag in site.tags %}
  <li class="listing-seperator" id="{{ tag[0] }}">{{ tag[0] }}</li>
{% for post in tag[1] %}
  <li class="listing-item">
  <time datetime="{{ post.date | date:"%Y-%m-%d" }}">{{ post.date | date:"%Y-%m-%d" }}</time>
  <a href="/Notes{{ post.url }}" title="{{ post.title }}" class="listing-item-a">{{ post.title }}</a>
  </li>
{% endfor %}
{% endfor %}
</ul>

<script src="/medias/js/jquery-1.7.1.min.js" type="text/javascript" charset="utf-8"></script> 
<script src="/medias/js/jquery.tagcloud.js" type="text/javascript" charset="utf-8"></script> 
<script language="javascript">
    $.fn.tagcloud.defaults = {
        size: {start: 14, end: 42, unit: 'px'},
        color: {start: '#ACE6E6', end: '#226666'}
    };
    $(function () {
    var recentColor, recentSize;
        $('#tag_cloud a')
            .tagcloud()
            .mouseover(function(){
                var thiz = $(this);
                recentColor = thiz.css('color');
                //recentSize = thiz.css('font-size');
                thiz.css({'color': '#226666'});
            })
            .mouseout(function(){
                $(this).css({'color': recentColor});
            });
    });
</script>
