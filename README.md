freeproxy
=========

FOSS Alternative to (OCLC's) EZProxy

It looks like OCLC is going to start charging an annual fee beginning in 2014.  I do not like this.  As such, I am looking to build a FOSS solution.

The plan is to use Apache as a base, with various scripts to handle adding databases and other various tidbits.

This post talks briefly how we're going to add DBs to the configuration: http://serverfault.com/questions/341696/open-source-free-alternative-to-ezproxy .

Authentication will probably be handled via PAM.  I'm not sure.  It's been a while since I've looked into user auth with Apache.

It'll be nice to have *everything* GUI driven from an admin interface.  We'll see how that goes.  I'll work on a flowchart.  Feel free to email me with any questions.  $USERNAME @ gmail.

I've also setup an IRC channel on freenode - #freeproxy .

I'd like to have a working product by the beginning/middle of July.  Hopefully people can deploy it come August and do testing during the Fall semester.

I'm looking forward to this project!

General Flow
=========
Right now I'm looking at maybe a hybrid Python/Apache instance.  All the bulk leg work will be relying on apache to proxy certain hosts.  An example configuration will be:
    <VirtualHost *:80>
        ServerName nature.com.proxy.myuniversity.edu
        ServerAlias www.nature.com.proxy.myuniversity.edu
        Order deny,allow
        Deny from all
        # Allow authorized IPs here:
        Allow from 10.0.0.0/8
        Allow from 172.16.0.0/12
        ProxyPreserveHost Off
        ProxyPass / http://www.nature.com/
        ProxyPassReverse / http://www.nature.com/
    </VirtualHost>

Each "stanza" will be a virtual host.  We'll have certain global configurations which we may be able to tie into an overlapping, config, then by using Apache's default method of reading in priveleges, go from there.

I wouldn't mind using Python for all the admin/configuration/user authentication stuff.  We can just slap that around a SimpleHTTPServer call and be set.  From there we can kick off conf changes in apache or make system calls to reload apache.  Whatever the case may be.  The bulk of what we'd need it to take care of may be there.  I'm not sure how we'll handle authentication.  I was thinking just use the Python stuff to set a cookie and be good to go.  Check the cookie in the Python web server, if it's good, pass the request on to Apache.  Not sure.  But then we may be restricted to *only* allowing "proxy by hostname".  We may have an issue with using cookies for auth.  Not sure.  We need to discuss this.

I'll work on a pretty DFD/flowchart.



faq
=========
**What license will be used?**

No idea.  Probably BSD.  GPL would be nice but it's still too early to ask for stuff back.  If people want to give it to us, by all means, feel free, but it shouldn't be mandatory.

**What method of proxying will be used?**

More than likely, only proxy by hostname.

**What platforms will be supported?**

I'm only planning on setting it up on Linux.  And even then, just CentOS/RHEL/SL.  It should be trivial to get it working on other 'nix platforms.  I'll need somebody to figure out the Windows side of things, if there's a demand.  I'll probably work on a "golden" virtual machine image that could just be shared out.  That seems like the better way to go about it.

**Why are you competing with EZProxy?**

We're not *really* competing with ezproxy in the traditional sense.  Instead, we're only trying to be good enough to provide competition to what's practically a monopoly.  OCLC hasn't really done much in the form of advancing EZProxy over the 5 years or so they've had it.  I think that's disrespectful to the users.  Maybe if we can show them what they *should* be doing all along, it'll give people a reason to pay an annual fee.  Not to mention, access to knowledge should be free.  We don't need a great, killer product, just a minimally viable product.


Features & Requests
=========
Distributed/Integrated Stanza List (possibly a drop down enable)

Easy to configure global settings.

Intuitive configuration settings.

LDAP Authentication (Active Directory)

Virtual machine image

RHEL/CentOS/Ubuntu/Debian repositories to stay up to date

relevant packages (.rpm, .deb)

Better/complete documentation

Got something you want to see?  Let us know!
