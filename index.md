### Posts
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
      
      {% if post.tags %}
        <small style="display: block">
          <div style="text-transform: lowercase">
            <!-- tags: <em>{{ post.tags | join: "</em>, <em>" }}</em> -->
            <em>{{ post.tags | join: ", " }}</em>
          </div>
          <div>
            {{ post.date | date: "%D %r" }}
          </div>
        </small>
      {% endif %}
            
      <div style="margin-right: 25px; text-align: justify">{{ post.excerpt }}</div>
    </li>
  {% endfor %}
</ul>
