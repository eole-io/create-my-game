## Allow more players to join

When creating a party, players can join it.

But they don't join the party directly. Then are assigned to a `Slot`.


### Slots

Slot are the "join" table between `Party` and `Player`.

It will contain:
 - the score of the player on this party,
 - the order of the player.

By default, each party created will create two slots:
 - one slot with the host assigned to,
 - another empty slot.

Once all slots are be joined by a player, party is full.

Unless you add more slots.


### Create more slots

If your game can be played by 4 players, you will need to add two extra slots.

To achieve this, you'll need to listen to the `PartyEvent::CREATE_BEFORE` event.

This event is dispatched during the party creation process, when someone click "Create a party".

``` php
<?php

namespace Eole\Games\MyGame;

use Eole\Core\Event\PartyEvent;
use Eole\Core\Service\PartyManager;

class PartyListener
{
    /**
     * @var PartyManager
     */
    private $partyManager;

    /**
     * @param PartyManager $partyManager
     */
    public function __construct(PartyManager $partyManager)
    {
        $this->partyManager = $partyManager;
    }

    /**
     * @param PartyEvent $event
     */
    public function onPartyCreateBefore(PartyEvent $event)
    {
        $eoleParty = $event->getParty();

        // Only do something when the party created event comes from "my-game"
        if ('my-game' !== $eoleParty->getGame()->getName()) {
            return;
        }

        // Add two empty slots (in addition to the two defaults ones)
        $eoleParty->addEmptySlot();
        $eoleParty->addEmptySlot();

        // Automatically set an order to slots (or the order of the new slot will be empty)
        $this->partyManager->reorderSlots($eoleParty);
    }
}
```


### Register your listener

Register this event to the Silex application.

It will be done on the RestApi stack,
as websocket stack won't be needed to listen to this event.

The logic here is to declare the listener class as a service,
then call the method on event triggered:

``` php
<?php

namespace Eole\Games\MyGame;

use Pimple\ServiceProviderInterface;
use Pimple\Container;
use Eole\Core\Event\PartyEvent;

class ControllerProvider implements ServiceProviderInterface
{
    /**
     * {@InheritDoc}
     */
    public function register(Container $app)
    {
        $app->before(function () use ($app) {
            $app->on(
                PartyEvent::CREATE_BEFORE,
                [$app['eole.games.my-game.listener.party'], 'onPartyCreateBefore']
            );
        });

        $app['eole.games.owls.listener.party'] = function () use ($app) {
            return new PartyListener($app['eole.party_manager']);
        };
    }
}
```

And don't forget to make your `GameProvider` provide the ControllerProvider:

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
    public function createControllerProvider()
    {
        return new ControllerProvider();
    }
}
```
