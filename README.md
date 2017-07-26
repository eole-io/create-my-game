## Create my game

Documentation to integrate a game using eole-api and eole-angular.


## Install Eole locally

You'll need

- git
- docker
- docker-compose

Clone both eole-api and eole-angular repositories.

``` bash
git clone git@github.com:eole-io/eole-api.git
git clone git@github.com:eole-io/eole-angular.git
```

Then, run `make` in each of them.

``` bash
cd eole-api/
make
cd ../

cd eole-angular/
make
cd ../
```

> **Note**:
> You'll probably need to run in `eole-api/`:
>
> `chown -R GROUP:USER .` and/or `chown -R 777 var/cache var/logs var/oauth-tokens`

The install is done.


## Default urls

Using default docker configuration, you'll have access to each of them urls:

What                               | url
---------------------------------- | -----------------------------
Eole front application             | [http://0.0.0.0:8580](http://0.0.0.0:8580)
Eole Api                           | [http://0.0.0.0:8480/api-docker.php/api/games](http://0.0.0.0:8480/api-docker.php/api/games)
Api Symfony profiler               | [http://0.0.0.0:8480/api-docker.php/_profiler/](http://0.0.0.0:8480/api-docker.php/_profiler/)
PHPMyAdmin (`root` / `root`)       | [http://0.0.0.0:8481](http://0.0.0.0:8481)
Websocket server                   | ws://0.0.0.0:8482


## Cookbook

1. [Initialize my game](initialize)

### Back

1. [Create a RestApi endpoint](create-rest-api-endpoint)
1. [Allow more players to join](allow-more-players-to-join)

### Front

1. [Calling Eole Api](calling-eole-api)
