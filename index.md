---
layout: frontpage
title: Home
---

<h2>The Hacker Project</h2>
<p>Project Nsmg is a hacker group that builds tools for hackers. There are great people that you can work together to build great things and amazing projects.</p>
<p>
  <a href="/about" class="btn">About Us</a>
  <a href="https://github.com/project-nsmg" class="btn">Github</a>
</p>

<hr>

<h2>Made in Project Nsmg</h2>
<p>Here are some projects that people have worked in Project Nsmg.</p>

<div class="themes">
  {% for post in site.collections.projects.docs %}
  {% unless post.archived %}
  <div class="theme">
    <h3>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </h3>
    <p>{{ post.description }}</p>
    <p>
      {% for link in post.links %}
        <a href="{{ link.url }}" class="btn">{{ link.title }}</a>
      {% endfor %}
    </p>
    </div>
  {% endunless %}
  {% endfor %}
</div>

<hr>

<h2>Essays</h2>
<p>Some interesting things people wrote.</p>

<ul class="related-posts">
  {% for post in site.collections.essays.docs %}
  <li>
    <h3>
      <a href="{{ post.url }}">
        {{ post.title }}
        <small>(<time datetime="{{ post.year | date_to_xmlschema }}">{{ post.year }}</time>)</small>
      </a>
    </h3>
  </li>
  {% endfor %}
</ul>

<hr>

<h2>Past Projects</h2>
<p>Something people in Project Nsmg have worked on in the past.</p>

<div class="themes">
  {% for post in site.collections.projects.docs %}
  {% if post.archived %}
  <div class="theme">
    <h3>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </h3>
    <p>{{ post.description }}</p>
    <p>
      {% for link in post.links %}
        <a href="{{ link.url }}" class="btn">{{ link.title }}</a>
      {% endfor %}
    </p>
    </div>
  {% endif %}
  {% endfor %}
</div>

<hr>

<p>&copy; {{ site.time | date: '%Y' }} Project Nsmg.</p>
