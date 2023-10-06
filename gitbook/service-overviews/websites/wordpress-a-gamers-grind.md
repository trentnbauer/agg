# Wordpress - A Gamers Grind

[Link to App](https://agamersgrind.com)

[Link to GitHub or Website](https://github.com/docker-library/wordpress)

WordPress is a web content management system. It was originally created as a tool to publish blogs but has evolved to support publishing other web content, including more traditional websites, mailing lists and Internet forum, media galleries, membership sites, learning management systems and online stores.

This app is hosted on Cocoa as a docker container

This stack is made up of 2 containers,

* MySQL DB
* Wordpress container

{% @github-files/github-code-block url="https://github.com/trentnbauer/agg.local/blob/main/docker-compose/wordpress.yml" %}

| Port | Purpose |
| ---- | ------- |
| 90   | HTTP    |

| Host Volume        | Container Volume | Purpose           |
| ------------------ | ---------------- | ----------------- |
| agg-wordpress\_app | /var/www/html    | website contents  |
| agg-wordpress\_db  | /var/lib/mysql   | database contents |

| Integration | Purpose |
| ----------- | ------- |
|             |         |

\
