# assg: ansible-based static site generator

I've always been a huge fan of keeping things simple. Static site generators always seemed pretty neat to me. Why have all that overhead of a database or a complex framework just to share some ideas? I'm also a huge fan of [ansible](https://www.ansible.com/) which bakes in the [jinja2](http://jinja.pocoo.org/) template engine. 

Since I've already been using ansible to deploy infrastucture and code anyway, I reckoned it wouldn't be that tough to statically generate a website with it and skip the middle-man. This is the result.


# usage

If it isn't there already, install ansible through the package manager of your choice.

Create an inventory file with a `webhost` group, containing the target webservers:

~~~
$ cat hosts 
[webhost]
mywebserver.mydomain.tld
~~~

Then run the playbook:
~~~
$ ansible-playbook -i hosts site.yml
~~~

This will generate a site with the examples specific in `roles/assg/vars/main.yaml`.

You might want to store site specific information along with blog posts in a separate YAML file. This way, you can manage multiple different sites using the same playbook, but with different content.

Assuming your site's configuration is stored as so:
~~~
$ cat mycheeseblog.yml
---
# vars file for mycheeseblog.com
site_title: mycheeseblog.com
site_url: http://mycheeseblog.com
bottom_links:
  -
    name: linkedin
    target: http://linkedin.com/me
  -
    name: twitter
    target: https://twitter.com/me
  -
    name: github
    target: https://github.com/me

top_links:
  -
    name: mycoolthing
    target: http://coolthing.org
  -
    name: myotherthing
    target: http://thingsrule.net


blog_posts:
  -
    title: 'eating cheese'
    date: '2017-08-24'
    body: |
      Cheese fest coming up! Can't wait to eat all that cheese.
      Stay tuned for updates!
  - 
    title: 'lactose intolerance'
    date: '2017-08-26'
    body: |
      Turns out I'm lactose intolerant. Not feeling so hot.
      Current mood: :(

about:
  body: |
    I love cheese!
    This is my blog about eating a lot of cheese.
~~~

You could share your love of cheese with the following:
~~~
$ ansible-playbook -i hosts site.yml -e @mycheeseblog.yml
~~~

There is also a 'hidden' option to purge the contents of the target director first. To do that, run with:

~~~
$ ansible-playbook -i hosts site.yml -e @mycheeseblog.yml -e purge=true
~~~

To add a new blog post, update the `blog_posts` parameter and re-run the playbook. Though it is idempotent and there is no harm in running the full thing each time, you can generate just new posts by using the `posts` tag:
~~~
$ ansible-playbook -i hosts site.yml -e @mycheeseblog.yml --tags posts
~~~

Finally, I included a role I use to deploy apache on RPM based systems. If you are using something else, or don't require a webserver, you can remove it from the playbook, or just skip it with tags:

~~~
$ ansible-playbook -i hosts site.yml -e @mycheeseblog.yml --skip-tags apache
~~~


# fork it
This spits out a minimalistic website that suits my tastes, but obviously everyone is different. Think of this playbook as a starting point, providing some of the plumbing to deploy static sites. Take it, mess with the templates and style sheets and all that and do your thing.

