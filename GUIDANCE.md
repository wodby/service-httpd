# Apache HTTP Server on Wodby

What Wodby sets up for this web server. Apache here is configured by environment variables, by the config files the service declares and by the application's own `.htaccess` files.

## How it is configured

On every start the container renders its configuration from templates and environment variables: `conf/httpd.conf`, `conf/conf.d/vhost.conf` (one virtual host) and `conf/preset.conf` under `/usr/local/apache2`. They are rewritten on each start; never edit them in a running container.

To change the configuration:

- Set `APACHE_*` environment variables on the service.
- Change the `docroot` setting.
- Override the virtual host template, declared as a config file of this service.
- Add `.htaccess` files to the codebase: overrides are allowed in the document root (`APACHE_ALLOW_OVERRIDE_ENABLED`), and `mod_rewrite`, `mod_headers`, `mod_expires` and `mod_deflate` are loaded.

## Preset

`APACHE_VHOST_PRESET` selects the rules included in the virtual host. This service uses the image default, `html`: files are served from the document root with `index.html` as the directory index (`APACHE_DIRECTORY_INDEX`). There is no fallback for paths that match no file; a single-page application needs its own rewrite rule in `.htaccess`. The image also ships a `php` preset, used by the PHP service built on this one.

## Document root

The codebase is at `/var/www/html`. The `docroot` setting (variable `DOCROOT_SUBDIR`) names a subdirectory of the repository, and `APACHE_DOCUMENT_ROOT` is set to `/var/www/html/` followed by it. Leave the setting empty to serve the repository root.

## Always present

- `/.healthz` returns 204 and is not logged. Use it for health checks.
- Files and directories whose names start with a dot are denied, as are files ending in `.sh`, `.sql`, `.mysql`, `.po`, `.tpl`, `.make` or `.test`, `wodby.yml` and `Makefile`.
- Directory listings are off unless `APACHE_INDEXES_ENABLED` is set.

## Variables that matter most

| Variable | Effect |
| --- | --- |
| `APACHE_DIRECTORY_INDEX` | directory index file |
| `APACHE_ALLOW_OVERRIDE_ENABLED` | what `.htaccess` files may override |
| `APACHE_TIMEOUT`, `APACHE_REQUEST_READ_TIMEOUT` | request time limits |
| `APACHE_LIMITED_ACCESS` | removes the rule that grants access to everyone, so that access is defined elsewhere |
| `APACHE_HTTP2` | accepts cleartext HTTP/2 |
| `APACHE_INCLUDE_CONF` | the files included in place of the generated virtual host |
| `APACHE_LOG_LEVEL` | error log level |

Access and error logs go to the container's standard output and error.

## Build and reaching the service

- The image is built from the connected repository: the codebase is copied into the image at `/var/www/html`. Files are served from the built image, so a change needs a new build and deployment.
- Other services reach it over HTTP on port 80 at the app service's name inside the environment.

## Check the result

- `httpd -S` prints the virtual host in effect; `httpd -t` checks the syntax.
- `curl -sI localhost/.healthz` returns 204 from inside the container.
