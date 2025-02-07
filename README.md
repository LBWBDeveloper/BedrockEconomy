# BedrockEconomy
BedrockEconomy is an economy plugin made for PocketMine-MP focused on stability and simplicity.
<br>

## Commands
| Name | Description | Usage | Permission |
| ------- | ----------- | ----- | ---------- |
| balance | Show your and others balance | `balance [player: string]` | `bedrockeconomy.command.balance` |
| pay | Pay others with your balance | `pay <player: string> <amount: number>`  | `bedrockeconomy.command.pay` |
| topbalance | View the top balances | `topbalance [page: number]` | `bedrockeconomy.command.topbalance` |
| addbalance | Add points to others balance | `addbalance <player: string> <amount: number>`  | `bedrockeconomy.command.addbalance` |
| removebalance | Remove points from others balance | `removebalance <player: string> <amount: number>`  | `bedrockeconomy.command.removebalance` |
| setbalance | Set others balance | `setbalance <player: string> <balance: number>`  | `bedrockeconomy.command.setbalance` |
| deleteaccount | Delete others account data | `deleteaccount <player: string>`  | `bedrockeconomy.command.deleteaccount` |

## API

### Get the balance of a player

```php
BedrockEconomyAPI::getInstance()->getPlayerBalance(
    "Steve",
    ClosureContext::create(
        function (?int $balance): void {
            var_dump($balance);
        },
    )
);
```

### Increment the balance of a player

```php
BedrockEconomyAPI::getInstance()->addToPlayerBalance(
    "Steve",
    1000,
    ClosureContext::create(
        function (bool $wasUpdated): void {
            var_dump($wasUpdated);
        },
    )
);
```

### Decrement the balance of a player

```php
BedrockEconomyAPI::getInstance()->subtractFromPlayerBalance(
    "Steve",
    1000,
    ClosureContext::create(
        function (bool $wasUpdated): void {
            var_dump($wasUpdated);
        },
    )
);
```

### Update the balance of a player

```php
BedrockEconomyAPI::getInstance()->setPlayerBalance(
    "Steve",
    1000,
    ClosureContext::create(
        function (bool $wasUpdated): void {
            var_dump($wasUpdated);
        },
    )
);
```

### Transfer money from one player to another

```php
BedrockEconomyAPI::getInstance()->transferFromPlayerBalance(
    "Steve", // Sender
    "Alex",  // Receiver
    1000,    // Amount
    ClosureContext::create(
        function (bool $successful): void {
            var_dump($successful);
        },
    )
);
```

### Check if a player has an account

```php
BedrockEconomyAPI::getInstance()->isAccountExists(
    "Steve",
    ClosureContext::create(
        function (bool $hasAccount): void {
            var_dump($hasAccount);
        },
    )
);
```

### Delete a player's account

```php
BedrockEconomyAPI::getInstance()->deletePlayerAccount(
    "Steve",
    ClosureContext::create(
        function (bool $operationSuccessful): void {
            var_dump($operationSuccessful);
        },
    )
);
```

### Get highest balances

```php
BedrockEconomyAPI::getInstance()->getHighestBalances(
    limit: 10,
    context: ClosureContext::create(
        function (?array $accounts) use ($sender, $offset): void {
            if (!$accounts) {
                var_dump("The table is empty.");
                return;
            }

            foreach ($accounts as $account) {
                var_dump($account["username"] . ": " . $account["balance"]);
            }
        }
    ),
    /**
     * Offset is used to skip the first n * limited results.
     * By default is set to 0 to retrieve the top n results starting from 0
     */
    offset: 1,
);
```

### Addons

```php
/**
 * The name of the addon, must be unique.
 *
 * @return string
 */
public function getName(): string;
````

```php
/**
 * The version of the addon.
 *
 * @return string
 */
public function getVersion(): string;
````

```php
/**
 * The minimum supported version of BedrockEconomy.
 * If the version is @link Addon::SUPPORTED_BEDROCK_VERSION_ALL, the addon will be enabled on all BedrockEconomy versions.
 *
 * @return string
 */
public function getMinimumSupportedBedrockEconomyVersion(): string;
````

```php
/**
 * Called before a plugin is enabled, this should be only used for dependency checking.
 *
 * @return bool
 */
public function isLoadable(): bool;
````

```php
/**
 * Returns whether the addon is enabled or not.
 *
 * @return bool
 */
public function isEnabled(): bool;
````

```php
/**
 * Called when the plugin is enabled. Similar to @link PluginBase::onEnable()
 * Should be used for listeners registration and such logic.
 */
public function onEnable(): bool;
````

```php
/**
 * Called when the addon is disabled. Similar to @link PluginBase::onDisable()
 */
public function onDisable(): bool;
````

### Events

| Name | Description |
| ------- | ----------- |
| TransactionSubmitEvent | Called right before a transaction is submitted to the database |
| TransactionProcessEvent | Called right after the transaction execution  |

#### Cancel a payment if the sender's name is Steve

```php
/**
 * @param TransactionSubmitEvent $event
 * @param TransferTransaction $transaction
 */
$transaction = $event->getTransaction();
if($transaction->getSender() === "Steve"){
    $event->cancel();
}
```

#### Broadcast a message to all players if a payment is successful

```php
/**
 * @param TransactionProcessEvent $event
 */
if($event->isSuccessful()){
    Server::getInstance()->broadcastMessage("A payment was successful!");
}
```

## Tools

* [Migration from EconomyAPI](https://github.com/cooldogedev/EconAPIToBE)

## Memes

<img width="250px" height="250px" src="https://user-images.githubusercontent.com/53002741/155570551-906c07ce-be6b-4d2e-87a2-a1fa733e05fb.png"  alt="Meme"/>
