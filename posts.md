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
