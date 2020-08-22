<h2 style="display: inline-block">./Posts</h2>
<h2 style="display: inline-block; margin-left: 10px">./Projects</h2>
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
      <small style="display: block; font-style: italic">
        ({{ post.tags | join: ", " }})
      </small>
      <div style="margin-right: 50px; text-align: justify">{{ post.excerpt }}</div>
    </li>
  {% endfor %}
</ul>
