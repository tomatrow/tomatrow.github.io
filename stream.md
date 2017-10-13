---
title: Stream
layout: default
category: main
---

# {{ page.title }}
<!-- We scale the stream size of 1440x900 (my Air's resolution) by 1/3. -->
{% assign streamHeight = 900 %}
{% assign streamWidth = 1440 %}
{% assign factor = 3 %}
{% assign height = streamHeight | divided_by: factor %}
{% assign width = streamWidth | divided_by: factor %}

<iframe
    src="http://player.twitch.tv/?channel=tomatrow&muted=true&autoplay=false"
    frameborder="0"
    scrolling="no"
    allowfullscreen="true"
    height="{{ height }}"
    width="{{ width }}">
</iframe>

<!-- We select the same width as the player. -->
{% assign size = width %}
<iframe src="http://www.twitch.tv/tomatrow/chat"
        id="chat_embed"
        frameborder="0"
        scrolling="no"
        height="{{ size }}"
        width="{{ size }}">
</iframe>