# List of posts

{% for post in site.posts %}
* post.title [{{ post.title }}]({{ post.url }})
{% endfor %}
