---
layout: post
title: Ruby on Rails - Stub Remote IP with RSpec
tags:
- rails
- rspec
- ruby
---
Today I wanted to simulate an external IP request with RSpec (2.8.0) and Rails
(3.1). Unfortunately Google didn't reveal much upon a search. So I came up with this solution:

{% gist 2941985 %}

The _**ActionDispatch**_ module holds the **_Request _**class which contains
information about the HTTP request, like the request body, authorization etc.
It also contains a method for obtaining the associated ip address obtained by
the remote ip middleware.

As I haven't had access to the request object, I used_** any_instance**_ to
tell RSpec that literally any derived instance from that class should get a
_**stub**_bed version of the _remote_ip_-method and then return a predefined ip address when that method is called.

**Update**

I also figured out that you can achieve that behavior by simply specifying
another parameter when making a request via RSpec:

{% gist 2941990 %}
