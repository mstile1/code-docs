# Docs and how-tos

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>

<img src="assets/mstile_avatar_768.webp" alt="avatar" width="256" height="256"/>

[Opening a port on a local network](network/open-port.html)

{% post_url 2023-01-20-strong-types %}
