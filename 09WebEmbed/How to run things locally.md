# How to run things locally


## Procedural content

If you use just procedural geometries and don't load any textures, webpages should work straight from the file system, just double-click on HTML file in a file manager and it should appear working in the browser (accessed as `file:///example`).

## Content loaded from external files

If you load models or textures from external files, due to browsers' "[same origin policy](http://en.wikipedia.org/wiki/Same_origin_policy)" security restrictions, loading from a file system will fail with a security exception.

There are two ways how to solve this:

1. Change security for local files in a browser (access page as `file:///example`)

2. Run files from a local server (access page as `http://localhost/example`)

If you use option 1, be aware that you may open yourself to some vulnerabilities if using the same browser for a regular web surfing. You may want to create a separate browser profile / shortcut used just for local development to be safe.

***

### Change local files security policy

#### Safari

Enable the develop menu using the preferences panel, under Advanced -> "Show develop menu in menu bar"

Then from the safari "Develop" menu, select "Disable local file restrictions", it is also worth noting safari has some odd behaviour with caches, so it is advisable to use the "Disable caches" option in the same menu; if you are editing & debugging using safari.

#### Chrome
Close all running Chrome instances first. The important word here is 'all'.

On Windows, you may check for Chrome instances using the Windows Task Manager. Alternatively, if you see a Chrome icon in the system tray, then you may open its context menu and click 'Exit'. This should close all Chrome instances.

Then start the Chrome executable with a command line flag:

```
chrome --allow-file-access-from-files
```

On Windows, probably the easiest is probably to create a special shortcut icon which has added the flag given above (right-click on shortcut -> properties -> target).

On Mac OSX, you can do this with
```
open /Applications/Google\ Chrome.app --args --allow-file-access-from-files
```

#### Firefox

1. Go to `about:config`
2. Find `security.fileuri.strict_origin_policy` parameter
3. Set it to `false`

***

### Run local server

The simplest probably is to use Python's built-in http server. 

If you have [Python](http://python.org/) installed, it should be enough to run this from a command line:

```bash
# Python 2.x
python -m SimpleHTTPServer
```

```bash
# Python 3.x
python -m http.server
```

This will serve files from the current directory at localhost under port 8000:

http://localhost:8000/

If you have Ruby installed, you can get the same result running this instead:

```bash
ruby -r webrick -e "s = WEBrick::HTTPServer.new(:Port => 8000, :DocumentRoot => Dir.pwd); trap('INT') { s.shutdown }; s.start"
```

PHP also has a built-in web server, starting with php 5.4.0:
```bash
php -S localhost:8000
```

Node.js has a simple HTTP server package. To install:
```bash
npm install http-server -g
```

To run:
```bash
http-server .
```

Other simple alternatives are [discussed here](http://stackoverflow.com/q/12905426/24874) on Stack Overflow.

Of course, you can use any other regular full-fledged web server like [Apache](http://www.apachefriends.org/en/xampp.html) or [nginx](http://nginx.org/).

Example with lighttpd, which is a very lightweight general purpose webserver (on MAC OSX):

 1. Install it via homebrew ``brew install lighttpd``
 2. Create a configuration file called lighttpd.conf in the directory where you want to run your webserver. There is a sample in [this](http://redmine.lighttpd.net/projects/lighttpd/wiki/TutorialConfiguration) page.
 3. In the conf file, change the server.document-root with the directory you want to serve
 4. Start it with ``lighttpd -f lighttpd.conf``
 5. Navigate to http://localhost:3000/ and it will serve static files from the directory you chose.
