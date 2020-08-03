---
layout: compress
---
<html lang="{{ page.lang | default: site.lang | default: "en-US" }}">

  {% include head.html %}

  <body class="site">

    {% if site.google_tag_manager %}

      <!-- Google Tag Manager (noscript) -->
      <noscript><iframe src="https://www.googletagmanager.com/ns.html?id={{ site.google_tag_manager }}"
      height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>
      <!-- End Google Tag Manager (noscript) -->

    {% endif %}

    {% include header.html %}

    <div class="hero--small">
      <div class="hero__wrap">
        <h1 class="hero__title">tldnr:/>grep {{ page.title | slugify }}</h1>
      </div>
    </div>
    <main class="site__content">
<section class="blog">
<div class="container">
<h1>Posts with tags including <a class="label" href="/tags/{{page.title|slugify}}">{{page.title|slugify}}</a>.</h1>
<ul class="posts">
  {% for post in page.posts %}
    <li>
      <span class="post-date">{{ post.date | date: "%b %-d, %Y" }}</span>
      <a class="post-link" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
</div>
<br>
</section>
    </main>

    {% include footer.html %}

  </body>

</html>

