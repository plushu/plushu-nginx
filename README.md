# plushu-nginx

Plugin to integrate Plushu with Nginx

## This plugin is (mostly) deprecated

Like [plushu-git][] and [plushu-docker][], this plugin works by integrating
with Nginx as installed in the host environment by the normal mechanisms of
the distro. Much of the plugin's code is dedicated to smoothly integrating with
the host system's (default) Nginx configuration, as described in the rest of
this documentation.

[plushu-git]: https://github.com/plushu/plushu-git
[plushu-docker]: https://github.com/plushu/plushu-docker

However, when possible, running daemons like Nginx in an encapsulated Docker
container environment is simpler and less fragile than running it in the
largely implementation-dependent and widely-varying host environment. For this
reason, this plugin has been superseded for general use (in [Plusku][]) by the
[plushu-nginx-container][] plugin, which uses the same configuration files (as
managed by other plugins) as this plugin, in a drop-in compatible fashion. The
only difference is the mechanism by which the Nginx server is provided and
started.

[Plusku]: https://github.com/plushu/plusku
[plushu-nginx-container]: https://github.com/plushu/plushu-nginx-container

This plugin should only be used to run Nginx if you *need* to integrate Plushu
into an existing host Nginx setup (eg. you're setting it up alongsite static
files being served by the distro's patched version of Nginx), or if you're
running some kind of Plushu configuration that doesn't involve plushu-docker.


## Installing

Installation will take the original configuration from /etc/nginx/nginx.conf
and write all directives outside of "http" blocks to
$PLUSHU_ROOT/nginx/nginx.conf. Any configuration *within* http blocks will be
written to a file named $PLUSHU_ROOT/nginx/http/existing.conf.save (or
successive files like $PLUSHU_ROOT/nginx/http/existing.1.conf.save, if your
nginx.conf uses multiple http blocks). You can, if you so choose, enable the
configuration directives they specify by removing the '.save' suffix. (Note
that you may have to copy files included by *those* configurations from
/etc/nginx.old to $PLUSHU_ROOT/nginx.)

(If you want to enable existing configurations *as separate http blocks*, you
should move their content back into $PLUSHU_ROOT/nginx/nginx.conf within a new
http block.)

## Using

As a plugin author, install files in the $PLUSHU_ROOT/nginx/http directory to
integrate your plugin's needed configuration changes.

As an end user, if you need to make non-Plushu changes specific to your
installation (and don't want to package them as a plugin), add directives
within the http block (such as server blocks) as changes within
$PLUSHU_ROOT/nginx/http/existing.conf, and directives outside of the http block
to $PLUSHU_ROOT/nginx/nginx.conf.
