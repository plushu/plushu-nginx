# plushu-nginx

Plugin to integrate Plushu with Nginx

## Installing

Installation will take the original configuration from /etc/nginx/nginx.conf
and write all directives outside of "http" blocks to
$PLUSHU_ROOT/nginx/nginx.conf. Any configuration *within* http blocks will be
written to a file named $PLUSHU_ROOT/nginx/http/existing.conf.save (or
successive files like $PLUSHU_ROOT/nginx/http/existing.1.conf.save, if your
nginx.conf uses multiple http blocks). You can, if you so choose, enable the
configuration directives they specify by removing the '.save' suffix. These
files will be used to restore your original nginx.conf if you later choose to
uninstall this plugin.

(If you want to enable them *as separate http blocks*, you should move their
content back into $PLUSHU_ROOT/nginx/nginx.conf within a new http block. This
will work correctly, and will be restored properly when uninstalling.)

## Using

As a plugin author, install files in the $PLUSHU_ROOT/nginx/http directory to
integrate your plugin's needed configuration changes.

As an end user, if you need to make non-Plushu changes specific to your
installation (and don't want to package them as a plugin), add directives
within the http block (such as server blocks) as changes within
$PLUSHU_ROOT/nginx/http/existing.conf, and directives outside of the Nginx
block to $PLUSHU_ROOT/nginx/nginx.conf.

Do *not* change the content of /etc/nginx/nginx.conf; any changes to the
expected content of the base configuration will cause uninstall to skip
restoration.

Likewise, do *not* change the line with the http block that includes
$PLUSHU_ROOT/nginx/http/*.conf: this line is the trigger for restoring
existing.conf http blocks.

## Uninstalling

Uninstalling will attempt to rewrite your original nginx.conf, using
$PLUSHU_ROOT/nginx/nginx.conf and any `existing.conf` (`.save`-suffixed or not)
files in the $PLUSHU_ROOT/nginx/http directory.

Note that, with the original structure of the $PLUSHU_ROOT/nginx/nginx.conf
that is created in installation, this will put any non-http-block directives
before the http block(s) restored from $PLUSHU_ROOT/nginx/http, whether they
were before, interspersed between, or after the http block(s) in the original
file.

Also note that, due to the way Plushu numbers existing.conf.save files when
creating them, if you had more then 10 http blocks in your original nginx.conf
(for some reason) and you want them restored in the same order they were
originally in, you will need to rename the files with leading zeroes, so that
they are properly sorted when listed in lexical order.
