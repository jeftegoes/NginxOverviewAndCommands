# 1. Commands

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
