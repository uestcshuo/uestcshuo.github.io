---
layout: page
title: 朝花夕拾
tagline: 朝花夕拾
---
{% include JB/setup %}

{% for post in site.posts %}
<div class = "card">
		<div  class = "date_label">
			<div class="day_month">
      			{{ post.date | date:"%m/%d" }}
      		</div>
      		<div class="year">
      			{{ post.date | date:"%Y" }}
      		</div>
      	</div> 
      	<a href="{{ BASE_PATH }}{{ post.url }}"><h2>{{ post.title }}</h2></a>
		{{ post.content  | | split:'<!--break-->' | first }}
	<div class = "read_more">
		<a class="fa fa-link" href="{{ BASE_PATH }}{{ post.url }}">阅读全文&hellip;</a>
	</div>
	
</div>

{% endfor %}

