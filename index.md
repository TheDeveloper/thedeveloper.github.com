---
layout: page
title: 
tagline: Supporting tagline
---
{% include JB/setup %}

### Posts

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

### About

<div class="row-fluid">
	<div class="span3">
	
	<img src="assets/images/profile.jpg" />
	
	</div>
	
	
	<div class="span6">
	Geoff Wagstaff is a 20 year old <a href="https://www.facebook.com/note.php?note_id=461505383919">full-stack</a> engineer and entrepreneur. He is co-founder and CTO of <a href="http://www.gosquared.com/">GoSquared</a>, a real-time web analytics service.
	
	<a href="https://twitter.com/TheDeveloper" class="twitter-follow-button" data-show-count="false" data-size="large">Follow @TheDeveloper</a>
<script>!function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0];if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src="//platform.twitter.com/widgets.js";fjs.parentNode.insertBefore(js,fjs);}}(document,"script","twitter-wjs");</script>
	</div>
</div>
