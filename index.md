---
layout: default
title: Home
---

## Welcome

Hi, I'm Jim This is my personal space for:

- 📝 **Blog** – Thoughts and insights
- 📚 **Learning** – Whatever rabbit hole I decided to go down, and how to apply it
- 📄 **Resume** – Professional..ish background

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
