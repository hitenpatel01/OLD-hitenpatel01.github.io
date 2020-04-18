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
            {{ post.date | date: "%-d %B %Y %h:%M %A" }}
          </div>
        </small>
      {% endif %}
            
      {{ post.excerpt }}
    </li>
  {% endfor %}
</ul>
