# Header1
## Header 2
### Posts
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
        <small style="display: flex">
          <span style="flex: 1 1 50%">{{ post.date | date: "%B %Y" }}</span>
          <span style="flex: 1 1 50%">{{ post.tags | join: "</em>, <em>" }}</span>
        </small>            
      <div style="margin-right: 25px; text-align: justify">{{ post.excerpt }}</div>
    </li>
  {% endfor %}
</ul>
