<a href="/posts"><h2 style="display: inline-block">./Posts</h2></a>
<a href="/projects" style="margin-left: 10px"><h2 style="display: inline-block">./Projects</h2></a>
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
