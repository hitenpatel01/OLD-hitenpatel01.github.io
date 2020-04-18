### Posts
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
        <small style="display: block; text-transform: lowercase">
          {{ post.date | date: "%d %Y" }}
          {% if post.tags %}    
            <span>: </span>
            <em>{{ post.tags | join: "</em>, <em>" }}</em>
          {% endif %}
        </small>            
      <div style="margin-right: 25px; text-align: justify">{{ post.excerpt }}</div>
    </li>
  {% endfor %}
</ul>
