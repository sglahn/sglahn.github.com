---
layout: page
---

<ul class="post-archives">
    {% assign year = 2000 %}
    {% for post in site.posts %}
    {% assign actYear = post.date | date: "%Y" %}
    {% if year != actYear %}
        {% assign year = actYear %}
        <br>
        <h2>{{ year }}</h2>
    {% endif %}
    <li>
        <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }} 
        <h2>
            <a class="post-link" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
        </h2>
        </span>
    </li>
    {% endfor %}
</ul>
