---
title: "Kubernetes"
layout: archive
permalink: categories/k8s
author_profile: true
sidebar: true
---

{% assign posts = site.categories.k8s %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}