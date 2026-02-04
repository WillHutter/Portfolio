---
layout: default
title: William deLone Hutter – Projects
permalink: /projects/
---

# Projects

Below is a selection of my engineering and design projects.

---

{% for project in site.projects reversed%}

## [{{ project.title }}]({{ project.url | relative_url }})

{% if project.description %}
{{ project.description }}
{% endif %}

{% if project.technologies %}
**Technologies:** {{ project.technologies | join: ", " }}
{% endif %}

<hr>

{% endfor %}
