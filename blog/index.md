---
layout: page
title: Moka's Blog ............
excerpt: "An archive of blog posts sorted by date."
search_omit: true
---
<ul class="post-list">

<h2 id="effective-android">effective-android</h2>
{% for post in site.categories.effective-android %}
<li>
    <article>
        <a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }} 
            <em class="entry-date" style="float: right;"><br>
                <span style="font-size: 11px">posted on </span>
                <time datetime="{{ post.date | date_to_xmlschema }}">
                    {{ post.date | date: "%B %d, %Y" }}
                </time>
            </em>
        </a>
    </article>
</li>
{% endfor %}

<h2 id="android">android</h2>
{% for post in site.categories.android %} 
<li>
    <article>
        <a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }} 
            <em class="entry-date" style="float: right;"><br>
                <span style="font-size: 11px">posted on </span>
                <time datetime="{{ post.date | date_to_xmlschema }}">
                    {{ post.date | date: "%B %d, %Y" }}
                </time>
            </em>
        </a>
    </article>
</li>
{% endfor %}

<h2 id="blog">blog</h2>
{% for post in site.categories.blog %} 
<li>
    <article>
        <a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }} 
            <em class="entry-date" style="float: right;"><br>
                <span style="font-size: 11px">posted on </span>
                <time datetime="{{ post.date | date_to_xmlschema }}">
                    {{ post.date | date: "%B %d, %Y" }}
                </time>
            </em>
        </a>
    </article>
</li>
{% endfor %}

<h2 id="javascript">javascript</h2>
{% for post in site.categories.javascript %} 
<li>
    <article>
        <a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }} 
            <em class="entry-date" style="float: right;"><br>
                <span style="font-size: 11px">posted on </span>
                <time datetime="{{ post.date | date_to_xmlschema }}">
                    {{ post.date | date: "%B %d, %Y" }}
                </time>
            </em>
        </a>
    </article>
</li>
{% endfor %}

<h2 id="study">study</h2>
{% for post in site.categories.study %} 
<li>
    <article>
        <a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }} 
            <em class="entry-date" style="float: right;"><br>
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
