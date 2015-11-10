# PSB - ProophServiceBus

PHP 5.5+ lightweight message bus supporting CQRS and Micro Services

[![Build Status](https://travis-ci.org/prooph/service-bus.png?branch=master)](https://travis-ci.org/prooph/service-bus)
[![Coverage Status](https://coveralls.io/repos/prooph/service-bus/badge.svg?branch=master&service=github)](https://coveralls.io/github/prooph/service-bus?branch=master)
[![Gitter](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/prooph/improoph)

## Messaging API

prooph/service-bus is a lightweight messaging facade.
It allows you to define the API of your model with the help of messages.

1. Command messages describe the actions your model can handle.
2. Event messages describe things that happened while your model handled a command.
3. Query messages describe available information that can be fetched from your model.

prooph/service-bus shields your model. Data input and output ports become irrelevant and no longer influence the business logic.
I'm looking at you Hexagonal Architecture.

prooph/service-bus decouples your model from any framework. You can use a
web framework like Zend, Symfony, Laravel and co. to handle http requests and pass them via prooph/service-bus to your model
but you can also receive the same messages via CLI or from a message queue system like RabbitMQ or Beanstalkd.

![prooph_architecture](https://raw.githubusercontent.com/prooph/proophessor/master/docs/book/img/prooph_overview.png)


## Installation

You can install prooph/service-bus via composer by adding `"prooph/service-bus": "~4.0"` as requirement to your composer.json.

## Quick Start

```php
<?php

use Prooph\ServiceBus\CommandBus;
use Prooph\ServiceBus\Example\Command\EchoText;
use Prooph\ServiceBus\Plugin\Router\CommandRouter;

$commandBus = new CommandBus();

$router = new CommandRouter();

//Register a callback as CommandHandler for the EchoText command
$router->route('Prooph\ServiceBus\Example\Command\EchoText')
    ->to(function (EchoText $aCommand) {
        echo $aCommand->getText();
    });

//Expand command bus with the router plugin
$commandBus->utilize($router);

//We create a new Command
$echoText = new EchoText('It works');

//... and dispatch it
$commandBus->dispatch($echoText);

//Output should be: It works
```

## Documentation

Documentation is [in the doc tree](docs/), and can be compiled using [bookdown](http://bookdown.io) and [Docker](https://www.docker.com/)

```console
$ docker run -it --rm -v $(pwd):/app sandrokeil/bookdown docs/bookdown.json
$ docker run -it --rm -p 8080:8080 -v $(pwd):/app php:5.6-cli php -S 0.0.0.0:8080 -t /app/docs/html
```

or make sure bookdown is installed globally via composer and `$HOME/.composer/vendor/bin` is on your `$PATH`.

```console
$ bookdown docs/bookdown.json
$ php -S 0.0.0.0:8080 -t docs/html/
```

Then browse to [http://localhost:8080/](http://localhost:8080/)

## Support

- Ask questions on [prooph-users](https://groups.google.com/forum/?hl=de#!forum/prooph) google group.
- File issues at [https://github.com/prooph/service-bus/issues](https://github.com/prooph/service-bus/issues).
- Say hello in the [prooph gitter](https://gitter.im/prooph/improoph) chat.

## Contribute

Please feel free to fork and extend existing or add new features and send a pull request with your changes!
To establish a consistent code quality, please provide unit tests for all your changes and may adapt the documentation.

## Dependencies

Please refer to the project [composer.json](composer.json) for the list of dependencies.

## License

Released under the [New BSD License](LICENSE).
