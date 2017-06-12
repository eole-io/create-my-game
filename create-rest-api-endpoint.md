## Create a RestApi endpoint

When developping the game backend, you will probably develop RestApi endpoints.

For example when a player perform a move, the backend will receive the move,
check game rules to know if this is an authorized move, then upate the game state.


### Game endpoint

All your game endpoints will be prefixed by `/api/games/{game}/`.

So if you create an endpoint `/play-move`, it will be available under `/api/games/my-game/play-move`.

> **Note**:
> You can list all endpoint with the command `bin/list-routes`.
>
> For example with docker:
> ``` bash
> # Run application
> make
>
> # Enter in container
> make bash
>
> # List routes
> bin/list-routes --env=docker
> ```


### Declare a Silex route

Eole Api is built on Silex.

Creating an endpoint is nothing more than declaring a Silex route.

[See how to create routes with Silex](https://silex.sensiolabs.org/doc/2.0/usage.html#example-get-route).



### Build the controller

You have first to create a `ControllerProviderInterface`:

``` php
<?php

namespace Eole\Games\MyGame;

use Alcalyn\SerializableApiResponse\ApiResponse;
use Silex\Api\ControllerProviderInterface;
use Silex\Application;

class ControllerProvider implements ControllerProviderInterface
{
    /**
     * {@InheritDoc}
     */
    public function connect(Application $app)
    {
        $controllers = $app['controllers_factory'];

        $controllers->post('/play-move', function (Application $app) {
            // Check the move
            // Play the move
            // Persist game state

            // Return a result to the client
            return new ApiResponse(null, 204);
        });

        return $controllers;
    }
}
```


### Register it in Eole

You will need to register it to Eole RestApi through your `GameProvider`:

``` php
<?php

namespace Eole\Games\MyGame;

use Eole\Silex\GameProvider;

class MyGame extends GameProvider
{
    /**
     * {@InheritDoc}
     */
    public function createControllerProvider()
    {
        return new ControllerProvider();
    }
}
```

Then check it is well registered with the `bin/list-routes` command,
or by calling it.

> **Note**:
> Example with docker:
>
> `POST http://0.0.0.0:8480/api-docker.php/api/games/my-game/play-move`
