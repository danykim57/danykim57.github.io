---
title: "설계"
layout: archive
permalink: categories/architecture
author_profile: true
sidebar: true
---

{% assign posts = site.categories.architecture %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}