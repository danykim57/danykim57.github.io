---
title: "디자인패턴"
layout: archive
permalink: categories/designPattern
author_profile: true
sidebar: true
---

{% assign posts = site.categories.designPattern %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}