---
title: Support
weight: 30
---

If you have any problems with [our out-of-tree drivers][digimend-kernel-drivers],
look for solutions and report new issues at [the issues page on
GitHub][issues]. Join the [#DIGImend channel on irc.freenode.net][irc_channel]
to discuss the drivers, tablets, development, to ask for help, and to help
others!

Read on for HOWTOs and other support articles.

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

[digimend-kernel-drivers]: https://github.com/DIGImend/digimend-kernel-drivers
[issues]: https://github.com/DIGImend/digimend-kernel-drivers/issues
[irc_channel]: https://webchat.freenode.net/?channels=DIGImend
