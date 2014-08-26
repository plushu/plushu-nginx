# plushu-nginx

Plugin to integrate Plushu with Nginx

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
