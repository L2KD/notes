In the http section of nginx.conf

Add this

    server_tokens off;

Restart nginx service

---

Bonus: Hide PHP version (Powered by...)

In php.ini

    expose_php = off

Restart php service
