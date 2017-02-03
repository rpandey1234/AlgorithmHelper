# Algorithm Helper (the good stuff)

{% for post in site.posts %}
  <h3><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></h3>
  {{ post.excerpt }}
  <p><b>{{ post.date | date: '%B %d, %Y' }} | {{ post.author }}</b></p>
  <hr>
{% endfor %}
