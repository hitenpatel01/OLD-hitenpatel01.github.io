<div style="display: flex">
  <div style="flex: 1 1 70%">
    <h2>./Recent Posts</h2>
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
  </div>
  <div style="flex: 1 1 30%">
    <h2>./Projects</h2>
    <ul>
      <li><a href="#">My Money Manager</a></li>
      <li><a href="#">SPA</a></li>
    </ul>
  </div>
</div>
