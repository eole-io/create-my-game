## Install Eole locally


### Requirements

You'll need

- git
- docker
- docker-compose


### Clone repositories and install docker environment

Clone both eole-api and eole-angular repositories.

Then, run `make` in each of them.

> **Note**:
> You'll probably need to run in `eole-api/`:
>
> `chown -R GROUP:USER .` and/or `chown -R 777 var/cache var/logs var/oauth-tokens`


### Use it

Using default docker configuration, you'll have access to each of them urls:

                                   | url
---------------------------------- | -----------------------------
Eole front application             | http://0.0.0.0:8580
Eole Api                           | http://0.0.0.0:8480/api-docker.php/api/games
Api Symfony profiler               | http://0.0.0.0:8480/api-docker.php/_profiler/
PHPMyAdmin (`root` / `root`)       | http://0.0.0.0:8481
Websocket server                   | ws://0.0.0.0:8482
