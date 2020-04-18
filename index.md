### Posts
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
      
      {% if page.tags %}
        <small>tags: <em>{{ page.tags | join: "</em> - <em>" }}</em></small>
      {% endif %}
            
      {{ post.excerpt }}
    </li>
  {% endfor %}
</ul>
