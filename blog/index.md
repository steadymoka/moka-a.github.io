---
layout: page
title:
excerpt: "An archive of blog posts sorted by date."
search_omit: true
---
<ul class="post-list">

<h2 id="android" style="border-bottom: 1px solid #CCCCCC; padding-left: 14px;">android</h2>
{% for post in site.categories.android %} 
<li style="border-bottom: 0px solid; margin-bottom: 12px">
    <article>
        <a href="{{ post.url | prepend: site.baseurl }}" style="font-size: 1.355rem">{{ post.title }}
            <em class="entry-date" style="float: none;"><br>
                <span style="font-size: 11px">posted on </span>
                <time datetime="{{ post.date | date_to_xmlschema }}">
                    {{ post.date | date: "%B %d, %Y" }}
                </time>
            </em>
        </a>
    </article>
</li>
{% endfor %}

<h2 id="blog" style="border-bottom: 1px solid #CCCCCC; padding-left: 14px;">blog</h2>
{% for post in site.categories.blog %} 
<li style="border-bottom: 0px solid; margin-bottom: 12px">
    <article>
        <a href="{{ post.url | prepend: site.baseurl }}" style="font-size: 1.355rem">{{ post.title }} 
            <em class="entry-date" style="float: none;"><br>
                <span style="font-size: 11px">posted on </span>
                <time datetime="{{ post.date | date_to_xmlschema }}">
                    {{ post.date | date: "%B %d, %Y" }}
                </time>
            </em>
        </a>
    </article>
</li>
{% endfor %}

<h2 id="etc" style="border-bottom: 1px solid #CCCCCC; padding-left: 14px;">etc</h2>
{% for post in site.categories.etc %} 
<li style="border-bottom: 0px solid; margin-bottom: 12px">
    <article>
        <a href="{{ post.url | prepend: site.baseurl }}" style="font-size: 1.355rem">{{ post.title }} 
            <em class="entry-date" style="float: none;"><br>
                <span style="font-size: 11px">posted on </span>
                <time datetime="{{ post.date | date_to_xmlschema }}">
                    {{ post.date | date: "%B %d, %Y" }}
                </time>
            </em>
        </a>
    </article>
</li>
{% endfor %}

<h2 id="effective-android" style="border-bottom: 1px solid #CCCCCC; padding-left: 14px;">effective-android</h2>
{% for post in site.categories.effective-android %}
<li style="border-bottom: 0px solid; margin-bottom: 12px">
    <article>
        <a href="{{ post.url | prepend: site.baseurl }}" style="font-size: 1.355rem">{{ post.title }} 
            <em class="entry-date" style="float: none;"><br>
                <span style="font-size: 11px">posted on </span>
                <time datetime="{{ post.date | date_to_xmlschema }}">
                    {{ post.date | date: "%B %d, %Y" }}
                </time>
            </em>
        </a>
    </article>
</li>
{% endfor %}

</ul>
