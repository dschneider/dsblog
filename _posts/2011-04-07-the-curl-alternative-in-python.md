---
layout: post
title: The cURL alternative in Python
tags:
- python
- curl
---
As I have been playing around with Python and Titanium a lot in the last few
weeks, I tried to use some APIs which use cURL to retrieve data. It took me
some time to figure out the cURL equivalent in Python - actually there is
PycURL, but it needs to be installed separately. But Python already comes with
an integrated solution called 'urllib2'.

I give you a small example based on the Lighthouse API.
[Lighthouse](http://lighthouseapp.com/) is a simple ticket tracking solution.

    
    #!/usr/bin/python
    
    import urllib2
    
    request = urllib2.Request(http://yourdomain.com/projects.xml)
    request.add_header(X-LighthouseToken, YOUR_TOKEN)
    response = urllib2.urlopen(request)
    data = response.read()
    print data
    

With that code you are able to fetch your projects from Lighthouse. Most of
the code is pretty much self-explanatory. If you want to do it using an
username and password instead of a token, you have to do it like that:

    
    #!/usr/bin/python
    
    import urllib2
    
    # create a password manager
    password_mgr = urllib2.HTTPPasswordMgrWithDefaultRealm()
    
    # Add the username and password.
    # If we knew the realm, we could use it instead of None.
    URL = http://yourdomain.com/projects.xml  
    password_mgr.add_password(None, URL, USERNAME, PASSWORD)
    
    handler = urllib2.HTTPBasicAuthHandler(password_mgr)
    
    # create "opener" (OpenerDirector instance)
    opener = urllib2.build_opener(handler)
    
    # use the opener to fetch a URL
    opener.open(URL)
    
    # Install the opener.
    # Now all calls to urllib2.urlopen use our opener.
    urllib2.install_opener(opener)
    
    request = urllib2.Request(URL)
    response = urllib2.urlopen(request)
    data = response.read()
    print data
    

What we do here, is to simply setup a password manager and a basic http
authentication handler which uses this password manager. The password manager
takes the target url, the username and the password as arguments. After that
we create an opener which will be used on every urllib2.urlopen()-call, so
every time we send a request. This opener takes the basic http auth handler as
an argument. After that you just fire your request using urllib2.urlopen(). Et
voilà you get your projects from Lighthouse again.

