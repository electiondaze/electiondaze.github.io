---
layout: default
title: Home
---

## Welcome

Hi, I'm **Your Name**. This is my personal space for:

- 📝 **Blog** – Thoughts and insights
- 📚 **Training** – Course materials and exercises
- 📄 **Resume** – Professional background

---

## Latest Posts

{% for post in site.posts limit:5 %}
### [{{ post.title }}]({{ post.url }})
<small>{{ post.date | date: "%B %d, %Y" }}</small>
{{ post.excerpt | strip_html | truncate: 100 }}
{% endfor %}

---

## Get in Touch

- Email: [your@email.com](mailto:your@email.com)
- GitHub: [@username](https://github.com/username)
- LinkedIn: [Your Profile](https://linkedin.com/in/username)
