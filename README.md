# Nginx overview and commands <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. File Structure and Directives](#2-file-structure-and-directives)
  - [2.1. Directives](#21-directives)
  - [2.2. Contexts](#22-contexts)
- [3. HTTP Block](#3-http-block)
- [4. Location Blocks](#4-location-blocks)
- [5. Commands](#5-commands)
- [6. SSL Configuration](#6-ssl-configuration)

# 1. Introduction

- **NGINX** is a lightweight and high-performance web server.
- Ideal for handling high traffic and serving static content.
- Uses an asynchronous model, unlike Apache’s process/thread-based model.

# 2. File Structure and Directives

- Main config file: `/etc/nginx/nginx.conf`
- Top-level (main) directives include: `user`, `worker_processes`, `error_log`, `pid`
- Every directive ends with a `;`, and comments begin with `#`
- Configuration blocks (also called contexts): `events`, `http`, `server`, `location`, etc.

## 2.1. Directives

```
  include       /etc/nginx/mime.types;
  default_type  application/octet-stream;
```

## 2.2. Contexts

```
  # Global Context

  # http Context
  http {

    # server Context
    server {

      # location Context
      localtion {

      }
    }
  }
```

# 3. HTTP Block

- Defines global settings for the entire server.
- **Common directives**
  - `include /etc/nginx/mime.types`
  - `access_log`, `log_format`
  - `sendfile`, `keepalive_timeout`
  - `include /etc/nginx/conf.d/*.conf`

# 4. Location Blocks

- Define rules based on URL paths or patterns.
- Types of location matching:

  - `location = /` = exact match.
  - `location ^~ /static/` = literal match with priority.
  - `location ~ \.php$` = case-sensitive regex.
  - `location ~\* \.jpg$` = case-insensitive regex.

- Example 1
  ```
    location / {
      root html;
      index index.html index.htm;
    }
  ```
- Example 2
  ```
    location / {
        return 200 "Hello, World!\n";
    }
  ```

# 5. Commands

- Start webserver
  - `start nginx`
- Fast shutdown
  - `nginx -s stop`
- graceful shutdown
  - `nginx -s quit`
- Changing configuration, starting new worker processes with a new configuration, graceful shutdown of old worker processes
  - `nginx -s reload`
- re-opening log files
  - `nginx -s reopen`
- Test if config file is OK
  - `nginx -t`

# 6. SSL Configuration

```
  events {}

  http {
      server {
          listen 80 default_server;
          server_name _;
          return 403;
      }

      server {
        listen 80;
          listen 443 ssl;

          server_name sub1.domain.com;

          ssl_certificate     /etc/ssl/cloudflare/domain.com.pem;
          ssl_certificate_key /etc/ssl/cloudflare/domain.com.key;

          location / {
              proxy_pass http://sub1:8080/;
          }
      }

      server {
        listen 80;
          listen 443 ssl;

          server_name sub2.domain.com;

          ssl_certificate     /etc/ssl/cloudflare/domain.com.pem;
          ssl_certificate_key /etc/ssl/cloudflare/domain.com.key;

          location / {
              proxy_pass http://sub2:8080/;
          }
      }
  }
```
