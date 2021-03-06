<!-- vim: set tw=79 sw=4 ts=4 et ft=markdown : -->
# Wrench
## Simple WebSocket Client/Server for PHP

* [![Latest Stable Version](https://poser.pugx.org/wrench/wrench/v/stable)](https://packagist.org/packages/wrench/wrench)
* [![Latest Unstable Version](https://poser.pugx.org/wrench/wrench/v/unstable)](https://packagist.org/packages/wrench/wrench)
* [![Build Status](https://secure.travis-ci.org/varspool/Wrench.png?branch=master)](http://travis-ci.org/varspool/Wrench)
* Documentation: [wrench.readthedocs.org](http://wrench.readthedocs.org/en/latest/index.html)

A simple websocket server and client package for PHP 7.1.

## Installation

The library is PSR-4 compatible, with a vendor name of **Wrench**. It's available on Composer as `wrench/wrench`, so you
can:

```sh
composer require wrench/wrench ~3.0
```

## Usage

This creates a server on 127.0.0.1:8000 with one Application that listens for
WebSocket requests to `ws://localhost:8000/echo` and `ws://localhost:8000/chat`:

### Server
```php
/**
 * An example application, that just echoes the received data back to the connection that sent it
 */
$app = new class implements \Wrench\Application\DataHandlerInterface
{
    public function onData(string $data, \Wrench\Connection $connection): void
    {
        $connection->send($data);
    }
};

// A websocket server, listening on port 8000
$server = new \Wrench\BasicServer('ws://localhost:8000', array(
    'allowed_origins' => array(
        'mysite.com',
        'mysite.dev.localdomain'
    )
));

$server->registerApplication('echo', $app);
$server->registerApplication('chat', new \My\ChatApplication());
$server->setLogger($monolog); // PSR3
$server->run();
```
### Client
```php
/**
 * A client side example, that sends a string and will receive the data back to the connection that sent it
 */
$client = new Client('ws://localhost:8000, http://localhost:8000);
$client->connect();
$client->sendData('hello');
$response = $client->receive()[0]->getPayload();
$client->disconnect();
```

## Releases and Changelog

See the [CHANGELOG](CHANGELOG.md) for detailed information about changes between releases.

### PHP5 Support

The latest major release dropped support for PHP versions prior to 7.1. If you need support for older versions of PHP,
see the 2.0 branch. The latest 2.0 branch release is 2.0.8. You can install it with:

```
composer require wrench/wrench ~2.0
```

## Developing

* Run `./vendor/bin/phpcs` and fix any errors that you come across
* Run `./vendor/bin/phpunit --exclude-group large,medium` for a fast test between changes

## See Also

- [Ratchet](https://github.com/cboden/Ratchet) an excellent Websocket layer for
  React.
