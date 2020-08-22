<h2 style="display: inline-block"><a href="/posts">[Posts]</a></h2>
<h2 style="display: inline-block"><a href="/projects">[Projects]</a></h2>
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
      <small style="display: block; font-style: italic">
        ({{ post.tags | join: ", " }})
      </small>
      <div>{{ post.excerpt }}</div>
    </li>
  {% endfor %}
</ul>
