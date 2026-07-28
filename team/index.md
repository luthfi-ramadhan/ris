---
title: Team
nav:
  order: 3
  tooltip: About our team
---

# {% include icon.html icon="fa-solid fa-users" %}Team

The Research and Innovation Services (RIS) is supported by a team of lecturers and researchers with expertise in various areas of high performance computing. Our team is responsible for supporting research activities, managing HPC resources, and providing technical assistance to researchers and students. 

{% include section.html %}

{% include list.html data="members" component="portrait" filter="role == 'pi'" %}
{% include list.html data="members" component="portrait" filter="role != 'pi'" %}


{% include grid.html style="square" content=content %}
