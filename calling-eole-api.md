## Calling Eole API

From front application, you will need to query your backend to:

 - retrieve party data (players, score...),
 - retrieve your party data (position of players, cards... depending on your game)
 - perform a move


### Eole Api JS client

Eole angular application provide the [eoleApi](https://github.com/eole-io/eole-angular/blob/dev/eole_components/eole-api-client/eole-api-client.js) service.

It contains generic calls to your game and party base data.

It can be used in controllers by injecting it:

``` js
/* global angular */

(function (angular) {
    'use strict';

    angular.module('eole.games.my-game').controller('my-game.PartyController', function (eoleApi) {
        //                                                           Inject eoleApi here ^

        // Access eoleApi here
    });
})(angular);
```

Then call [eoleApi](https://github.com/eole-io/eole-angular/blob/dev/eole_components/eole-api-client/eole-api-client.js) methods, in exemple to access current party base data:

``` js
/* global angular */

(function (angular) {
    'use strict';

    angular.module('eole.games.my-game').controller('my-game.PartyController', function (eoleApi, $scope, $routeParams) {
        $scope.party = null;

        eoleApi.getParty('my-game', $routeParams.partyId).then(function (party) {
            $scope.party = party;
        });
    });
})(angular);
```
