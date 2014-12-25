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

[1]: mailto:digimend-users@lists.sourceforge.net
[2]: http://sourceforge.net/p/digimend/mailman/digimend-users/
[3]: https://lists.sourceforge.net/lists/listinfo/digimend-users

Miscellaneous
-------------
<dl>
{% assign misc_dir = page.url | remove: 'index.html' | append: 'misc/' %}
{% include pi parent_dir=misc_dir include="pt_d.html" %}
</dl>

Maillist
--------
We maintain a general user support maillist at
[digimend-users@lists.sourceforge.net][1]. Please search [the archives][2]
before posting. You will be able to receive an answer if you just send a
message there, but you can also [subscribe][3], if you wish to receive
messages sent by others.
