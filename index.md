### Posts
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
      
      {% if post.tags %}
        <small>tags: <em>{{ post.tags | join: "</em> - <em>" }}</em></small>
      {% endif %}
            
      {{ post.excerpt }}
    </li>
  {% endfor %}
</ul>
