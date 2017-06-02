## Initialize my game

Once you [installed Eole environment](./install), you'll have to develop a backend and a frontend.


### Game folders

Game can be developped in:

- **Backend**: `eole-api/src/Eole/Games/MyGame/`
- **Frontend**: `eole-angular/eole/games/my-game/`

Note that there is already a folder "my game" in both backend and frontend.

You can start from them to avoid to initialize the structure, or create a new folder.

If you create a new one, see below.


### Backend

Your backend will be Silex ServiceProvider and ControllerProvider.

Your game backend will provide:

- Services layer and database model extension
- API endpoints (i.e playing a move, get party information, ...)
- Websocket Topic (for realtime push notifications, i.e someone played his turn...)


#### Game provider

Each game extends the Silex stack by providing a GameProvider class, which extends [Eole\Silex\GameProvider](https://github.com/eole-io/eole-api/blob/dev/src/Eole/Silex/GameProvider.php):

``` php
<?php

namespace Eole\Games\MyGame;

use Eole\Core\Model\Game;
use Eole\Silex\GameProvider;

class MyGame extends GameProvider
{
    /**
     * {@InheritDoc}
     */
    public function createGame()
    {
        $game = new Game();

        $game->setName('my-game');

        return $game;
    }
}
```

The `createGame()` method has to create an instance of [Eole\Core\Model\Game](https://github.com/eole-io/eole-api/blob/dev/src/Eole/Core/Model/Game.php).


#### Register game in config

In `eole-api/config/environment.yml`, add a new `mod`:

``` yml
mods:
    # ...
    my-game:
        provider: Eole\Games\MyGame\MyGame
```

Then, register game in Eole games list so that it will be added to the database:

``` bash
cd eole-api/

# enter in php container
make bash

php bin/console --env=docker eole:games:install
```

The game should now be returned by the RestApi by going to [/api/games](http://0.0.0.0:8480/api-docker.php/api/games).


### Frontend

Your frontend will be an angular module and a npm module (for deployment).

It will provide one or more angular routes, views, and angular controllers.


#### npm module

Create an `index.js` file in `eole-angular/eole/games/my-game/`
(it will contains all assets required by your game required for deployment):

``` js
var path = require('path');

module.exports = {
    assets: {
        css: [
            path.resolve(__dirname, 'css/*.css')
        ],
        templates: [
            'views/*.html'
        ],
        js: [
            path.resolve(__dirname, 'module.js'),
            path.resolve(__dirname, 'routes.js'),
            path.resolve(__dirname, 'controllers/*.js'),
            path.resolve(__dirname, 'translations/*.js')
        ],
        files: [
            [path.resolve(__dirname, 'images/*.*'), 'images/'],
            [path.resolve(__dirname, 'fonts/*.*'), 'fonts/']
        ]
    }
};
```


#### Angular module

Create a `module.js` file in `eole-angular/eole/games/my-game/`:

``` js
/* global angular */

(function (angular) {
    'use strict';

    angular.module('eole.games.my-game', ['ngRoute']);
})(angular);
```

> **Note**:
> The module name has to be prefixed by `eole.games.`
> because of technical contraints from the deployment process.


#### Create the game main page

When someone will create or join a party of the game,
he will be redirected to `/games/my-game/parties/:partyId`.

This will be the main page of the game.
Up to you to make it a lobby page, display the game board...

``` js
/* global angular */

(function (angular) {
    'use strict';

    angular.module('eole.games.my-game').config(function ($routeProvider, gamePath) {
        $routeProvider.when('/games/my-game/parties/:partyId', {
            controller: 'my-game.PartyController',
            templateUrl: gamePath+'/views/index.html'
        });
    });
})(angular);
```

Then create the controller `controllers/party.js`:

``` js
/* global angular */

(function (angular) {
    'use strict';

    angular.module('eole.games.my-game').controller('my-game.PartyController', function ($scope, $routeParams) {
        var gameName = $routeParams.gameName;
        var partyId = $routeParams.partyId;

        $scope.hello = 'Hello world !';
    });
})(angular);
```

And the view `views/index.html`:

``` html
<div class="container">
    <h2>{{ 'my-game'|translate }}</h2>
    <h3>{{ hello }}</h3>
</div>
```


#### Test your game

You should now be able to create a party of your game, and let people join your party.
