Role Name
=========

An ansible role to generate a static web site.

Requirements
------------

N/A

Role Variables
--------------

In `defaults/main.yml` you'll find all the important variables. There are some sample blog entries in there too, but you might want to overwrite them with a `-e @blog_post.yml` argument to `ansible-playbook`. You can also set `purge=true` (via cli or in a yml) to clear out the `document_root` directory prior to site generation.

~~~
document_root: /var/www/html
site_title: my site
site_url: http://www.my.blog
blog_posts:
  -
    title: 'first post'
    date: '1979-01-01'
    body: |
      First post!
      Static sites are the best.
  - 
    title: 'last post'
    date: '2049-12-31'
    body: |
      Goodbye cruel world.
about:
  body: |
    My name is mud.
    I'm in to this and that.
    I like cheese.

bottom_links:
  -
    name: linkedin
    target: http://www.linkedin.com
  -
    name: twitter
    target: https://twitter.com/
  -
    name: github
    target: https://github.com/

top_links:
  -
    name: projectA
    target: http://github.com
  -
    name: projectB
    target: http://github.com
~~~

Dependencies
------------

No hard dependencies, but you might want to run with the optional httpd module if you aren't using one already and need those bits setup.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: webhost
      become: true
      roles:
      - ../roles/httpd
      - ../roles/assg

License
-------

BSD

Author Information
------------------

David Critch (dcritch@gmail.com)
