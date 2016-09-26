#  :sparkles: Ultimate developer / designer collection  :sparkles:

[![Awesome](https://cdn.rawgit.com/sindresorhus/awesome/d7305f38d29fed78fa85652e3a63e154dd8e8829/media/badge.svg)](https://github.com/sindresorhus/awesome)

A continuously expanded collection of front-end resources, best practices, deployment tips, server configurations, security & resources
All idea's, feature requests, pull requests, feedback, etc., are welcome. [Create an issue](https://github.com/johanneslamers/Deployment-guide-for-developers/issues).

## Table of Contents

<!-- TOC depthFrom:1 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [:sparkles: Ultimate developer / designer collection  :sparkles:](#sparkles-ultimate-developer-designer-collection-sparkles)
	- [Table of Contents](#table-of-contents)
	- [Quick web development checklist](#quick-web-development-checklist)
		- [Fit and finish](#fit-and-finish)
		- [Mobile](#mobile)
		- [Analytics](#analytics)
		- [Accessibility](#accessibility)
		- [Code quality](#code-quality)
		- [Performance](#performance)
		- [Semantics](#semantics)
		- [SEO](#seo)
		- [Server & Security](#server-security)
	- [HTTP](#http)
		- [:key: HTTP Security Headers](#key-http-security-headers)
		- [HTTP Cache Headers](#http-cache-headers)
	- [Nginx](#nginx)
		- [Nginx tuning - making your servers fly :rocket:](#nginx-tuning-making-your-servers-fly-rocket)
			- [Worker Processes and Worker Connections](#worker-processes-and-worker-connections)
- [One worker per CPU-core.](#one-worker-per-cpu-core)
			- [HTTP and TCP Optimizations](#http-and-tcp-optimizations)
			- [Buffer size](#buffer-size)
			- [Timeouts](#timeouts)
			- [Gzip compression](#gzip-compression)
			- [PHP Tuning](#php-tuning)
- [first appeared in Nginx 1.5.6 (1.6.0 stable) and can be used to turn buffering completely on/off. It's on by default.](#first-appeared-in-nginx-156-160-stable-and-can-be-used-to-turn-buffering-completely-onoff-its-on-by-default)
- [A special buffer space used to hold the first part of the FastCGI response, which is going to be the HTTP response headers.](#a-special-buffer-space-used-to-hold-the-first-part-of-the-fastcgi-response-which-is-going-to-be-the-http-response-headers)
- [You shouldn't need to adjust this from the default - even if Nginx defaults to the smallest page size of 4KB (your platform will determine if 4/8k buffer) it should fit your typical HTTP header.](#you-shouldnt-need-to-adjust-this-from-the-default-even-if-nginx-defaults-to-the-smallest-page-size-of-4kb-your-platform-will-determine-if-48k-buffer-it-should-fit-your-typical-http-header)
- [The one exception I have seen are frameworks that push large amounts of cookie data via the Set-Cookie HTTP header during their user verification/login phase - blowing out the buffer and resulting in a HTTP 500 error. In those instances you will need to increase this buffer to 8k/16k/32k to fully accommodate your largest upstream HTTP header being pushed.](#the-one-exception-i-have-seen-are-frameworks-that-push-large-amounts-of-cookie-data-via-the-set-cookie-http-header-during-their-user-verificationlogin-phase-blowing-out-the-buffer-and-resulting-in-a-http-500-error-in-those-instances-you-will-need-to-increase-this-buffer-to-8k16k32k-to-fully-accommodate-your-largest-upstream-http-header-being-pushed)
- [So a total of 8 buffer segments at either 4k/8k, which is determined by the platform memory page size. For Debian/Ubuntu Linux that turns out to be 4096 bytes (4K) - so a default total buffer size of 32KB.](#so-a-total-of-8-buffer-segments-at-either-4k8k-which-is-determined-by-the-platform-memory-page-size-for-debianubuntu-linux-that-turns-out-to-be-4096-bytes-4k-so-a-default-total-buffer-size-of-32kb)
- [If you response size average tips on the higher side you might want to alternatively lower the buffer segment count and raise the memory size in page size multiples (8k/16k/32k).](#if-you-response-size-average-tips-on-the-higher-side-you-might-want-to-alternatively-lower-the-buffer-segment-count-and-raise-the-memory-size-in-page-size-multiples-8k16k32k)
- [Limits the size of data written to a temporary file at a time, when buffering of responses from the FastCGI server to temporary files is enabled. By default, size is limited by two buffers set by the fastcgi_buffer_size and fastcgi_buffers directives. The maximum size of a temporary file is set by the fastcgi_max_temp_file_size directive.](#limits-the-size-of-data-written-to-a-temporary-file-at-a-time-when-buffering-of-responses-from-the-fastcgi-server-to-temporary-files-is-enabled-by-default-size-is-limited-by-two-buffers-set-by-the-fastcgibuffersize-and-fastcgibuffers-directives-the-maximum-size-of-a-temporary-file-is-set-by-the-fastcgimaxtempfilesize-directive)
			- [Logging](#logging)
			- [Tip: Watching your logs](#tip-watching-your-logs)
- [grep "/login.php??" /var/log/nginx/mysite.com-access](#grep-loginphp-varlognginxmysitecom-access)
- [grep "...etc/passwd" /var/log/nginx/mysite.com-access](#grep-etcpasswd-varlognginxmysitecom-access)
- [egrep -i "denied|error|warn" /var/log/nginx/mysite.com-error](#egrep-i-deniederrorwarn-varlognginxmysitecom-error)
			- [Static asset serving & caching](#static-asset-serving-caching)
			- [Extra: Block Referral Spam](#extra-block-referral-spam)
			- [Extra: Block image hotlinking](#extra-block-image-hotlinking)
- [Stop deep linking or hot linking](#stop-deep-linking-or-hot-linking)
		- [Optimizing SSL on Nginx](#optimizing-ssl-on-nginx)
	- [PHP](#php)
		- [PHP Opcache](#php-opcache)
	- [Tools & Services](#tools-services)
		- [Security](#security)
		- [Webmasters](#webmasters)
		- [Analytics](#analytics)
		- [Browser Testing](#browser-testing)
		- [Monitoring & logging](#monitoring-logging)
		- [Optimization](#optimization)
		- [Structured Data](#structured-data)
		- [Keywords](#keywords)
		- [Links](#links)
		- [Bookmarklets & Browser Extensions](#bookmarklets-browser-extensions)
		- [Browser Extensions](#browser-extensions)
		- [Good Reads](#good-reads)
	- [How to Contribute](#how-to-contribute)
	- [Feedback](#feedback)
	- [License](#license)

<!-- /TOC -->

-----
## Quick web development checklist

### Fit and finish
- [ ] **Fix broken links**
- [ ] **Spell check**
- [ ] **Cross browser testing**
- [ ] **HTML5 compatibility**
- [ ] **Custom 404 pages**
- [ ] **Generate Favicon**
- [ ] **Forms tested**
- [ ] **Friendly URL's**
- [ ] **Duplicated content**

### Mobile
- [ ] **Mobile score 75+**
- [ ] **'Viewport' meta tag**
- [ ] **Correct input types**
- [ ] **Manual check**
- [ ] **Check using emulators**

### Analytics
- [ ] **Uptime monitoring**
- [ ] **Traffic Analytics**
- [ ] **Block your IP on Analytics**
- [ ] **Google webmaster tools**

### Accessibility
- [ ] **Accessibility validation**
- [ ] **Color contrast**
- [ ] **WAI-ARIA**

### Code quality
- [ ] **HTML Validation**
- [ ] **CSS Validation**
- [ ] **CSS Lint**
- [ ] **JS Lint**

### Performance
- [ ] **Page speed score 85+**
- [ ] **Yahoo YSlow score 85+**
- [ ] **Web page test score 90+**
- [ ] **Varvy Page Speed Optimization test**
- [ ] **Optimize HTTP Headers**
- [ ] **Optimize static file cache**
- [ ] **Optimize images**
- [ ] **HTTP2**

### Semantics
- [ ] **Add meaning with Microdata - JSON-LD**
- [ ] **Validate semantics**

### SEO
- [ ] **SenSEO score 85+**
- [ ] **Varvy SEO test**
- [ ] **robots.txt**
- [ ] **XML Sitemap**

### Server & Security
- [ ] **Install LetsEncrypt SSL**
- [ ] **Use HTTPS everywhere**
- [ ] **Patch LOGJAM Vulnerability**
- [ ] **Set HTTP Security Headers**
- [ ] **Set up monitoring for your systems, and log stuff (use New Relic or something like that)**
- [ ] **SSL Labs server test score "A"**
- [ ] **Enforce or remove WWW**
- [ ] **Add to HSTS preload list**
- [ ] **Backups**
- [ ] **Maintenance plan**






----

## HTTP

### :key: HTTP Security Headers

- **Content Security Policy (CSP) header.** :100:

    ````
    Content-Security-Policy "default-src https: data: 'unsafe-inline' 'unsafe-eval'" always;
    ````

- **HTTP Strict Transport Security (HSTS) header** :100:

    ````
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains; preload;"
    ````
- **X-Frame-Options header.** This is an HTTP header that allows sites control over how your site may be framed within an iframe
- **X-Content-Type-Options header.** This tells not to load scripts and stylesheets unless the server indicates the correct MIME type
- **X-XSS-Protection header.** Stops pages from loading when they detect reflected cross-site scripting (XSS) attacks

    ````
    X-XSS-Protection: 1; mode=block
    ````



### HTTP Cache Headers
- **Cache-Control**

    The Cache-Control general-header field is used to specify directives that MUST be obeyed by all the caching system. The syntax is as follows:
    An HTTP client or server can use the Cache-control general header to specify parameters for the cache or to request certain kinds of documents from the cache. The caching directives are specified in a comma-separated list. For example: ``Cache-control: no-cache``. The following table lists the important cache request directives that can be used by the client in its HTTP request:

    | #             | Cache Request Directive and Description    |
    | ------------- |-------------|
    | 1             | `no-cache`. A cache must not use the response to satisfy a subsequent request without successful revalidation with the origin server. |
    | 2             | `no-store`. The cache should not store anything about the client request or server response.      |
    | 3             | `max-age` = seconds. Indicates that the client is willing to accept a response whose age is not greater than the specified time in seconds.     |
    | 4             | `max-stale` = seconds. Indicates that the client is willing to accept a response that has exceeded its expiration time. If seconds are given, it must not be expired by more than that time.     |
    | 5             | `min-fresh` = seconds. Indicates that the client is willing to accept a response whose freshness lifetime is not less than its current age plus the specified time in seconds.     |
    | 6             | `no-transform`. Does not convert the entity-body.    |

- **Pragma**

    The old “pragma” header accomplishes many things most of them characterised by newer implementations. We are however most concerned with the `pragma: no-cache` directive which is interpreted by newer implementations as `cache-control: no-cache`. You don’t need to be concerned about this directive because it’s a request header which will be ignored by KeyCDN’s edge servers. It is however important to be aware of the directive for the overall understanding. Going forward, there won’t be new HTTP directives defined for pragma.

- **Expires**

    A couple of years back, this was the main way of specifying when assets expires. Expires is simply a basic date-time stamp. It’s fairly useful for old user agents which still roam unchartered territories. It is however important to note that cache-control headers, max-age and s-maxage still take precedence on most modern systems. It’s however good practice to set matching values here for the sake of compatibility. It’s also important to ensure you format the date properly or it might be considered as expired.

    ````
    Expires: Sun, 03 May 2015 23:02:37 GMT
    ````

    To avoid breaking the specification, avoid setting the date value to more than a year.

----

## Nginx

### Nginx tuning - making your servers fly :rocket:


####Worker Processes and Worker Connections

Nginx uses a fixed number of workers, each of which handles incoming requests. The general rule of thumb is that you should have one worker for each CPU-core your server contains.
You can count the CPUs available to your system by running: `grep processor /proc/cpuinfo | wc -l`

The ~worker_connections~ command tells our worker processes how many people can simultaneously be served by Nginx. The default value is 768; however, considering that every browser usually opens up at least 2 connections/server, this number can half. This is why we need to adjust our worker connections to its full potential. We can check our core's limitations by issuing a ulimit command: `ulimit -n`

With a duo-core processor this would give you a configuration that started like so:

*File excerpt: /etc/nginx/nginx.conf*
````
# One worker per CPU-core.
worker_processes  2;

events {
    worker_connections  1024;
    multi_accept        on;
    use                 epoll;
}

worker_rlimit_nofile 40000;
````

####HTTP and TCP Optimizations

Keep alive allows for fewer reconnections from the browser.

*File excerpt: /etc/nginx/nginx.conf*
````
http {
    # optimizes serving static files from the file system, like logos.
    keepalive_timeout  65;
    keepalive_requests 100000;

    # optimizes serving static files from the file system, like logos.
    sendfile           on;

    #allows Nginx to make TCP send multiple buffers as individual packets.
    tcp_nopush         on;

    # optimizes the amount of data sent down the wire at once by activating the TCP_CORK option within the TCP stack. TCP_CORK blocks the data until the packet reaches the MSS, which is equal to the MTU minus the 40 or 60 bytes of the IP header.
    tcp_nodelay        on;


    # Your content here ..
}
````

> - Here we've also raised the worker_connections setting, which specifies how many connections each worker process can handle.
> - The maximum number of connections your server can process is the result of: ~worker_processes * worker_connections (= 2048 in our example)~. In this case, we can server **2048 clients/second**. This is, however, even further mitigated by the keepalive_timeout directive.
> - We've enabled multi_accept which causes nginx to attempt to immediately accept as many connections as it can, subject to the kernel socket setup.
> - Finally use of the epoll event-model is generally recommended for best throughput.

####Buffer size

Another incredibly important tweak we can make is to the buffer size. If the buffer sizes are too low, then Nginx will have to write to a temporary file causing the disk to read and write constantly. There are a few directives we'll need to understand before making any decisions.

`client_body_buffer_size`: This handles the client buffer size, meaning any POST actions sent to Nginx. POST actions are typically form submissions.

`client_max_body_size`:sets the max body buffer size. If the size in a request exceeds the configured value, the 413 (Request Entity Too Large) error is returned to the client. For reference, browsers cannot correctly display 413 errors. Setting size to 0 disables checking of client request body size.

`client_header_buffer_size`: Similar to the previous directive, only instead it handles the client header size. For all intents and purposes, 1K is usually a decent size for this directive.

`large_client_header_buffers`: Shows the maximum number and size of buffers for large client headers. 4 headers with 4k buffers should be sufficient here.

`output_buffers` sets the number and size of the buffers used for reading a response from a disk. If possible, the transmission of client data will be postponed until Nginx has at least the set size of bytes of data to send. The zero value disables postponing data transmission.

*File excerpt: /etc/nginx/nginx.conf*
````
client_body_buffer_size       128K;
client_max_body_size           20m;
client_header_buffer_size       1k;
large_client_header_buffers   4 4k;
output_buffers               1 32k;
````

####Timeouts

Timeouts can also drastically improve performance.

`client_body_timeout` and `client_header_timeout` directives are responsible for the time a server will wait for a client body or client header to be sent after request. If neither a body or header is sent, the server will issue a 408 error or Request time out.

`keepalive_timeout` assigns the timeout for keep-alive connections with the client. Simply put, Nginx will close connections with the client after this period of time.

`send_timeout` is established not on the entire transfer of answer, but only between two operations of reading; if after this time client will take nothing, then Nginx is shutting down the connection.

*File excerpt: /etc/nginx/nginx.conf*
````
client_body_timeout         3m;
client_header_timeout       3m;
keepalive_timeout           3m;
send_timeout                3m;
````

####Gzip compression

Gzip can help reduce the amount of network transfer Nginx deals with. However, be careful increasing the gzip_comp_level too high as the server will begin wasting cpu cycles.

````
gzip_vary                    on;
gzip_proxied                any;
gzip_comp_level               2;
gzip_buffers              16 8k;
gzip_http_version           1.1;
gzip_types: text/html application/x-javascript text/css application/javascript text/javascript text/plain text/xml application/json application/vnd.ms-fontobject application/x-font-opentype application/x-font-truetype application/x-font-ttf application/xml font/eot font/opentype font/otf image/svg+xml image/vnd.microsoft.icon;
````

####PHP Tuning

Since disk is slow and memory is fast the aim is to get as many FastCGI responses passing through memory only. On the flip side we don't want to set an excessively large buffer as they are created and sized on a per request basis (i.e. it's not shared memory).


*file excerpt: /etc/nginx/nginx.conf*
````
# first appeared in Nginx 1.5.6 (1.6.0 stable) and can be used to turn buffering completely on/off. It's on by default.
fastcgi_buffering                 on;

# A special buffer space used to hold the first part of the FastCGI response, which is going to be the HTTP response headers.
# You shouldn't need to adjust this from the default - even if Nginx defaults to the smallest page size of 4KB (your platform will determine if 4/8k buffer) it should fit your typical HTTP header.
# The one exception I have seen are frameworks that push large amounts of cookie data via the Set-Cookie HTTP header during their user verification/login phase - blowing out the buffer and resulting in a HTTP 500 error. In those instances you will need to increase this buffer to 8k/16k/32k to fully accommodate your largest upstream HTTP header being pushed.
fastcgi_buffer_size             32k;

# So a total of 8 buffer segments at either 4k/8k, which is determined by the platform memory page size. For Debian/Ubuntu Linux that turns out to be 4096 bytes (4K) - so a default total buffer size of 32KB.
Based on the maximum/average response sizes determined above we can now raise/lower these values to suit. I typically keep buffer size at the default (memory page size) and adjust only the buffer segment count to a value for keep the bulk/all responses handled fully in buffer RAM. The default memory page size (in bytes) can be determined by the following command: `$ getconf PAGESIZE`

# If you response size average tips on the higher side you might want to alternatively lower the buffer segment count and raise the memory size in page size multiples (8k/16k/32k).
fastcgi_buffers               32 32k;
fastcgi_busy_buffers_size     32 32k;

# Limits the size of data written to a temporary file at a time, when buffering of responses from the FastCGI server to temporary files is enabled. By default, size is limited by two buffers set by the fastcgi_buffer_size and fastcgi_buffers directives. The maximum size of a temporary file is set by the fastcgi_max_temp_file_size directive.
fastcgi_temp_file_write_size  32 32k;
````

####Logging

The access_log portion defines the directive, the log_file portion defines the location of the access.log file, the log_format portion can be defined using variables (default format is combined). If you don’t require this information then it’s preferable to disable it, which will save your environment a bunch of additional processing and hard drive space too. The following shows an example of what the “combined” log_format looks like:

````
log_format combined '$remote_addr - $remote_user [$time_local]  '
    '"$request" $status $body_bytes_sent '
    '"$http_referer" "$http_user_agent"';
````

Therefore, when defining each portion of the access log directive, it may resemble the following:

````
access_log /var/log/nginx/mysite.com-access.log log_file combined;
````

####Tip: Watching your logs

They will give you some understanding of what attacks is thrown against the server and allow you to check if the necessary level of security is present or not.

````
# grep "/login.php??" /var/log/nginx/mysite.com-access
# grep "...etc/passwd" /var/log/nginx/mysite.com-access
# egrep -i "denied|error|warn" /var/log/nginx/mysite.com-error
````

Tip: You can install the open source tool [GoAccess](https://goaccess.io/) to analyize your logs. Or use the online tool https://papertrailapp.com/ for frustration-free log management.


####Static asset serving & caching

If your site serves static assets (such as CSS/JavaScript/images), Nginx can cache these files for a short period of time. Adding this within your configuration block tells Nginx to cache 1000 files for 30 seconds, excluding any files that haven’t been accessed in 20 seconds, and only files that have 5 times or more. If you aren’t deploying frequently you can safely bump up these numbers higher.

*File excerpt: /etc/nginx/nginx.conf*
````
open_file_cache max=1000 inactive=20s;
open_file_cache_valid 30s;
open_file_cache_min_uses 5;
open_file_cache_errors off;
````

It's possible to set expire headers for files that don't change and are served regularly. This directive can be added to the actual Nginx server block.

*File excerpt: /etc/nginx/nginx.conf*
````
location ~* (?:^|/)\. {
    deny all;
}

location ~* (?:\.(?:bak|config|sql|fla|psd|ini|log|sh|inc|swp|dist)|~)$ {
    deny all;
}

location ~* \.(?:manifest|appcache|html?|xml|json)$ {
    try_files $uri /index.php?$query_string;
    expires -1;
}

location ~* \.(?:rss|atom)$ {
    expires 1h;
    add_header Cache-Control "public";
}

location ~* \.(?:jpg|jpeg|gif|png|ico|cur|gz|svg|svgz|mp4|ogg|ogv|webm|htc)$ {
    try_files $uri /index.php?$query_string;
    expires 1M;
    add_header Cache-Control "public";
    etag off;
    access_log off;
    log_not_found off;
}

location ~* \.(?:css|js)$ {
    try_files $uri /index.php?$query_string;
    expires 1y;
    add_header Cache-Control "public";
    etag off;
    access_log off;
    log_not_found off;
}

location ~* \.(?:ttf|ttc|otf|eot|woff)$ {
    try_files $uri /index.php?$query_string;
    expires 1y;
    add_header Cache-Control "public";
    etag off;
    access_log off;
    log_not_found off;
}

location ~ /\.ht {
    deny all;
}
````

####Extra: Block Referral Spam

Inside the `nginx.conf` file add an HTTP map like the one below, adding an entry for each of the spam domains. Value of **1** is considered to be **spam**, **0** for **legitimate** traffic.
The `~*` before each domain means case-insensitive matching.

````
http {
    map $http_referer $bad_referer {
        default                  0;
        "~*spamdomain1.com"       1;
        "~*spamdomain2.com"       1;
        "~*spamdomain3.com"       1;
    }
}

server {    
    location / {
        # Check to see if the $bad_referer variable is set to 1 (true), and if so return an HTTP status code of 444 which is a 'No Response'. No information is returned to the spammer. It also will use less server resources.
        if ($bad_referer) {
            return 444;
        }
    }
}

````
Now to test that blocking is actually working we can run the following curl command: `curl -e "http://spamdomain1.com" "http://yoursite.com"` Which should return a message like: `curl: (52) Empty reply from server`

####Extra: Block image hotlinking

Image or HTML hotlinking means someone makes a link to your site to one of your images, but displays it on their own site. I strongly suggest you block and stop image hotlinking at your server level itself.

````
# Stop deep linking or hot linking
location ~ .(gif|png|webm|jpe?g)$ {
    valid_referers none blocked ~.google. ~.bing. ~.yahoo. server_names ~($host);
    if ($invalid_referer) {
        return   403;
    }
}
````


### Optimizing SSL on Nginx
- **Patch LOGJAM Vulnerability**
- **HTTP/2** - In your site’s Nginx configuration file add `listen 443 ssl http2;` to the end of the listen line for the server block
- **HTTP Strict Transport Security header** :100:

    ````
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains"
    ````
- **Connection Credentials Caching**

    ````
    ssl_session_cache shared:SSL:20m;
    ssl_session_timeout 180m;
    ````
    > This will create a cache shared between all worker processes. The cache size is specified in bytes (in this example: 20 MB). According to the Nginx documentation can 1MB store about 4000 sessions, so for this example, we can store about 80000 sessions, and we will store them for 180 minutes. If you expect more traffic, increase the cache size accordingly.

- **Disable SSL** (What? Yes!)

    Techically SSL (Secure Sockets Layer) is actually superseded by TLS (Transport Layer Security). I guess it is just out of old habit and convention we still talk about SSL. SSL contains several weaknesses, there have been various attacks on implementations and it is vulnerable to certain protocol downgrade attacks.

    The only browser or library still known to mankind that doesn’t support TLS is of course IE 6. Since that browser is dead (should be, there is not one single excuse in the world), we can safely disable SSL.

    The most recent version of TLS is 1.2, but there are still modern browsers and libraries that use TLS 1.0.

    So, we’ll add this line to our config then:

    ````
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ````
- **Optimizing the cipher suites**

    The cipher suites are the hard core of SSL/TLS. This is where the encryption happens, and I will really not go into any of that here. All you need to know is that there are very secure suits, there are unsafe suites and if you thought browser compatibility issues were big on the front-end, this is a whole new ballgame. Researching what cipher suites to use, what not to use and in what order takes a huge amount of time to research. Luckily for you, I’ve done it.

    First you need to configure Nginx to tell the client that we have a preferred order of available cipher suites:

    ````
    ssl_prefer_server_ciphers on;
    ````

    Next we have to provide the actual list of ciphers:

    ````
    ssl_ciphers 'ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-DSS-AES128-GCM-SHA256:kEDH+AESGCM:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-DSS-AES128-SHA256:DHE-RSA-AES256-SHA256:DHE-DSS-AES256-SHA:DHE-RSA-AES256-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:AES:CAMELLIA:DES-CBC3-SHA:!aNULL:!eNULL:!EXPORT:!DES:!RC4:!MD5:!PSK:!aECDH:!EDH-DSS-DES-CBC3-SHA:!EDH-RSA-DES-CBC3-SHA:!KRB5-DES-CBC3-SHA';
    ````





---

## PHP

### PHP Opcache
- **First find total amount of PHP files for 'opcache.max_accelerated_files'**

    ````
    $ find project/ -iname *.php|wc -l
    ````
- **Change OpCache configuration**

    ````
    opcache.memory_consumption=128
    opcache.max_accelerated_files=10000
    opcache.max_wasted_percentage=10
    opcache.validate_timestamps=0
    ````

---



---

## Tools & Services

### Security
- **[Mozilla Observatory](https://observatory.mozilla.org/)** :100:
- **[Security Headers](https://securityheaders.io/)**
- **[SSL Labs](https://www.ssllabs.com/ssltest/)**
- **[HSTS preload website submit](https://hstspreload.appspot.com/)** :100:

### Webmasters
- **[Bing Webmasters](http://www.bing.com/toolbox/webmaster)**
- **[Google Search Console (GWT)](https://www.google.com/webmasters/)**
- **[Google Tag Manager](https://www.google.com/analytics/tag-manager/)**

### Analytics
- **[Ahrefs](https://ahrefs.com/)**
- **[BuzzSumo](https://app.buzzsumo.com/research/most-shared)**
- **[Followerwonk](https://moz.com/followerwonk)**
- **[Google Analytics](https://www.google.com/analytics/)**
- **[Open Site Explorer](https://moz.com/researchtools/ose/)**
- **[Piwik](https://piwik.org/)**
- **[SEMrush](https://www.semrush.com/)**
- **[SEOstats](https://github.com/eyecatchup/SEOstats)**
- **[SimilarWeb](https://www.similarweb.com/)**
- **[Twitter Analytics](https://analytics.twitter.com/)**
- **[Yahoo Web Analytics](https://web.analytics.yahoo.com)**

### Browser Testing
- **[Browserling](https://www.browserling.com/)**

### Monitoring & logging
- **[GoAccess](https://goaccess.io/)**
- **[Papertrail](https://papertrailapp.com/)** :100:

### Optimization
- **[PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/)**
- **[Varvy Seo tool](https://varvy.com/)**
- **[Webpagetest.org](https://www.webpagetest.org/)**
- **[WooRank](https://www.woorank.com/)**

### Structured Data
- **[Google+ Snippet Creator](https://developers.google.com/+/web/snippet/)**
- **[Structured Data Testing Tool](https://developers.google.com/structured-data/testing-tool/)** :100:
- **[Twitter card validator](https://cards-dev.twitter.com/validator)**
- **[Facebook Debugger](https://developers.facebook.com/tools/debug)**
- **[Pinterest](https://developers.pinterest.com/rich_pins/validator/)**

### Keywords
- **[AdWords Keyword Tool](https://adwords.google.com/KeywordPlanner)**
- **[Google Trends](https://www.google.com/trends/)**
- **[Keyword Tool](http://keywordtool.io/)**
- **[Rankscanner](https://www.rankscanner.com/)**

### Links
- **[OpenLinkProfiler](http://www.openlinkprofiler.org/)**
- **[Screaming Frog SEO Spider Tool & Crawler Software](https://www.screamingfrog.co.uk/seo-spider/)**
- **[Search Engine Spider Simulator](http://tools.seochat.com/tools/search-spider-simulator)**

### Bookmarklets & Browser Extensions
- **[OuiSEO](https://github.com/carlsednaoui/seo-bookmarklet)**
- **[SEO Bookmarklet](https://twkm.ca/projects/seo-bookmarklet)**

### Browser Extensions
- **[BuiltWith Technology Profiler](https://chrome.google.com/webstore/detail/builtwith-technology-prof/dapjbgnjinbpoindlpdmhochffioedbn?hl=en)**
- **[Dimensions - a tool to measure screen dimensions](https://chrome.google.com/webstore/detail/dimensions/baocaagndhipibgklemoalmkljaimfdj?hl=en)**
- **[MozBar](https://moz.com/tools/seo-toolbar)**
- **[SenSEO for firefox](https://addons.mozilla.org/en-US/firefox/addon/senseo/)**

### Good Reads
- **[Servers for hackers](https://serversforhackers.com/)** :100:
- **[Scott Helme](https://scotthelme.co.uk/)** :100:

----


## How to Contribute

Please read [CONTRIBUTING](/CONTRIBUTING.md).


## Feedback

All idea's, feature requests, pull requests, feedback, etc., are welcome. [Create an issue](https://github.com/johanneslamers/Deployment-guide-for-developers/issues).

## License

[![CC0](http://mirrors.creativecommons.org/presskit/buttons/88x31/svg/cc-zero.svg)](https://creativecommons.org/publicdomain/zero/1.0/)
see [the `LICENSE` file](https://github.com/johanneslamers/Deployment-guide-for-developers/blob/master/LICENSE).
