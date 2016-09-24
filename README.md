

## :sparkles: The Security Checklist :sparkles:


##### Server Security
- [ ] Use HTTPS everywhere.
- [ ] Patch LOGJAM Vulnerability.
- [ ] Content Security Policy (CSP) header. https://wiki.mozilla.org/Security/Guidelines/Web_Security#Content_Security_Policy

    **Examples**

    Disable unsafe inline/eval, only allow loading of resources (images, fonts, scripts, etc.) over https (recommended)

    ```Content-Security-Policy: default-src https:```

    Disable the use of unsafe inline/eval, allow everything else

    ```Content-Security-Policy: *```

    Disable unsafe inline/eval, only load resources from same origin, except also allow images on imgur

    ```Content-Security-Policy: default-src 'self'; img-src 'self' https://i.imgur.com```

    Disable unsafe inline/eval, only load resources from same origin, fonts from google, images from same origin and imgur

    ```Content-Security-Policy: default-src 'self'; font-src 'https://fonts.googleapis.com'; img-src 'self' https://i.imgur.com```

    Pre-existing site uses too much inline code to fix, but wants to ensure resources are loaded only over https

    ```Content-Security-Policy: default-src https: 'unsafe-eval' 'unsafe-inline'```

- [ ] HTTP Strict Transport Security (HSTS) header

    ```add_header Strict-Transport-Security "max-age=31536000; includeSubDomains; preload"```

- [ ] X-Content-Type-Options header. This tells not to load scripts and stylesheets unless the server indicates the correct MIME type
- [ ] X-Frame-Options header. This is an HTTP header that allows sites control over how your site may be framed within an iframe.
- [ ] X-XSS-Protection header. Stops pages from loading when they detect reflected cross-site scripting (XSS) attacks.

    ``` # Block pages from loading when they detect reflected XSS attacks
    
    X-XSS-Protection: 1; mode=block ```






##### Optimizing SSL on nginx
- [ ] Patch LOGJAM Vulnerability
- [ ] HTTP Strict Transport Security header.

    ```add_header Strict-Transport-Security "max-age=31536000; includeSubDomains"```

- [ ] HTTP/2 - In your site’s nginx configuration file add ```listen 443 ssl http2;``` to the end of the listen line for the server block


#### HTTP Headers






##### Server testing
- [ ] https://www.ssllabs.com/ssltest/
- [ ] https://observatory.mozilla.org/ :100:


>


## Contributions wanted


## Feedback

All idea's, feature requests, pull requests, feedback, etc., are welcome. [Create an issue](https://github.com/johanneslamers/Deployment-guide-for-developers/issues).

## License

MIT License: see [the `LICENSE` file](https://github.com/johanneslamers/Deployment-guide-for-developers/blob/master/LICENSE).
