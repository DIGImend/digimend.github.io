---
title: Support
weight: 30
---

X.org driver setup
------------------
<dl>
{% assign howto_drivers_dir = page.url | remove: 'index.html' | append: 'howto/drivers/' %}
{% include pi parent_dir=howto_drivers_dir orderby="weight" include="pt_d.html" %}
</dl>

Application setup
-----------------
<dl>
{% assign howto_apps_dir = page.url | remove: 'index.html' | append: 'howto/apps/' %}
{% include pi parent_dir=howto_apps_dir include="pt_d.html" %}
</dl>

Troubleshooting
---------------
<dl>
{% assign howto_trbl_dir = page.url | remove: 'index.html' | append: 'howto/trbl/' %}
{% include pi parent_dir=howto_trbl_dir include="pt_d.html" %}
</dl>

Miscellaneous
-------------
<dl>
{% assign misc_dir = page.url | remove: 'index.html' | append: 'misc/' %}
{% include pi parent_dir=misc_dir include="pt_d.html" %}
</dl>
