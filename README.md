YiiAMQP
=======

YiiAMQP is a fully functional AMQP producer and conusumer Yii application component.

##Requirements

Tested with Yii version 1.1.13


##Quick Start

Install via composer, then configure your application to use this component by adding and updating to match your needs the following configuration

```php

'components' => array(
        'rabbitMQ' => array(
            'class' => 'YiiAMQP\AppComponent',
            'server' => array(
                'host' => 'localhost',
                'port' => '5672',
                'vhost' => '/',
                'user' => 'guest',
                'password' => 'guest'
            )
        ),
```


##Usage

### Producer

```php
Yii::app()->rabbitMQ->createConnection();
Yii::app()->rabbitMQ->declareQueue('mail');
Yii::app()->rabbitMQ->declareExchange('exchange.mailService', 'topic');
Yii::app()->rabbitMQ->bind('mail', 'exchange.mailService', 'mail');
Yii::app()->rabbitMQ->setQoS('0', '1', '0');
Yii::app()->rabbitMQ->sendJSONMessage('"test":"test"','mail');
Yii::app()->rabbitMQ->sendTextMessage('text message"','mail');
```

### Consumer

Initialise the component

```php
Yii::app()->rabbitMQ->declareExchange('exchange.mailService', 'topic');
Yii::app()->rabbitMQ->bind($queue, 'exchange.mailService', 'mail');
Yii::app()->rabbitMQ->setQoS('0', '1', '0');
Yii::app()->rabbitMQ->registerCallback(array($this, 'myCallback'));
Yii::app()->rabbitMQ->consume($queue, $this->id);
Yii::app()->rabbitMQ->wait();
```

Create the callback function

```php
public static function myCallback($msg) { }
```

##Contributing
Please submit all pull requests against *-wip branches. Thanks!

##Bug tracker
If you find any bugs, please create an issue at [https://github.com/mteichtahl/YiiAMQP/issues](https://github.com/mteichtahl/YiiAMQP/issues)

##Credits

- php-amqplib [https://github.com/videlalvaro/php-amqplib] Vadim Zaliva lord@crocodile.org
- rabbitMQ [http://www.rabbitmq.com/] VMWare

##License
[![License](http://i.creativecommons.org/l/by-sa/3.0/88x31.png)](http://creativecommons.org/licenses/by-sa/3.0/)
This work is licensed under a [Creative Commons Attribution-ShareAlike 3.0 Unported License](http://creativecommons.org/licenses/by-sa/3.0/)
