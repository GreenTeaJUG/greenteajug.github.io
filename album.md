---
layout: page
title: Album
permalink: /album/
---

### Photo
<div class="posts">
    {% for image in site.static_files %}
        {% if image.path contains 'images/photo' %}
            <img src="{{ site.baseurl }}{{ image.path }}" alt="image" width="32%"/>
        {% endif %}
    {% endfor %}
</div>

### Video
<div class="posts">
    {% for video in site.static_files %}
        {% if video.path contains 'images/video' %}
            <iframe src="{{ site.baseurl }}{{ video.path }}" frameborder="0" width="32%" sandbox></iframe>
        {% endif %}
    {% endfor %}
</div>
