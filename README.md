## The Security Checklist

##### Server Security
- [ ] Use HTTPS everywhere.
- [ ] Patch LOGJAM Vulnerability.



##### Optimizing SSL on nginx
- [ ] Patch LOGJAM Vulnerability
- [ ] HTTP Strict Transport Security header.
      add_header Strict-Transport-Security "max-age=31536000; includeSubDomains"

- [ ] HTTP/2 - This is by far the simplest step. In your site’s nginx configuration file add “http2” to the end of the listen line for the server block.

      listen 443 ssl http2;






##### Server testing
- [ ] https://www.ssllabs.com/ssltest/
- [ ] https://observatory.mozilla.org/
