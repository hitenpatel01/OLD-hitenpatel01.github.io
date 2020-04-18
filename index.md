### Posts
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
        <small style="display: block">
          {{ post.date | date: "%B %Y" }}
          {% if post.tags %}    
            <span>-</span>
            <span style="text-transform: lowercase">
              <em>{{ post.tags | join: "</em>, <em>" }}</em>
            </span>
          {% endif %}
        </small>            
      <div style="margin-right: 25px">{{ post.excerpt }}</div>
    </li>
  {% endfor %}
</ul>
