<h2 style="display: inline-block"><a href="/">/Home</a></h2>
<h2 style="display: inline-block; margin-left: 10px"><a href="/projects">/Projects</a></h2>
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
