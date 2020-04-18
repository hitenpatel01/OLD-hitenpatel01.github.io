<div style="display: flex">
  <div style="flex: 1 1 60%">
    ## Recent Posts
    <ul>
      {% for post in site.posts %}
        <li>
          <a href="{{ post.url }}">{{ post.title }}</a>
          <small style="display: block">
            {{ post.tags | join: ", " }}
          </small>
          <div style="margin-right: 50px; text-align: justify">{{ post.excerpt }}</div>
        </li>
      {% endfor %}
    </ul>
  </div>
  <div style="flex: 1 1 40%">
    ## Projects
    <ul>
      <li><a href="#">My Money Manager</a></li>
      <li><a href="#">SPA</a></li>
    </ul>
  </div>
</div>
