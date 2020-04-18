### Posts
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
        <small style="display: block">
          {{ post.date | date: "%B %Y" }}
          {% if post.tags %}    
            <span>-</span>
            <em style="text-transform: lowercase">{{ post.tags | join: "</em>, <em>" }}</em>
          {% endif %}
        </small>            
      <div style="margin-right: 25px; text-align: justify">{{ post.excerpt }}</div>
    </li>
  {% endfor %}
</ul>
