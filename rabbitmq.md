# RABBITMQ

## QUESTIONS
* MassTransit vs Rabbit?
* clarify queues vs exchanges
* what's a saga?


## ADMIN CONSOLE
* rabbitmq.com: documentation, plugin(like admin console)
* protocall: amqp is default
* overview

* queues
* features: "D" is durable -- it will always live there
* exchanges: messages for different exchanges map can map to one queue; exchange gets a message into a queue
	- fanout pattern is default



## Creating a Chat App with RabbitMQ and MassTransit
* specify a host, receiving endpoint that listens, concurrency limit
* then you start the service
* busControl publishes a message, comsumer received the message according to the channel configuration
* the individual chat user/chat instance is the consumer


