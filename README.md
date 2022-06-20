# machine test kafka
Machine means the local machine you are using

# docker-compose up
```shell
$ docker-compose up -d
```

# Add brokers hosts to your /etc/hosts file
```
##
# Host Database
#
# localhost is used to configure the loopback interface
# when the system is booting.  Do not change this entry.
##
127.0.0.1       localhost machine-broker1 machine-broker2 machine-broker3
...
```
# NodeJS Example
## Producer Example
```js
import { Kafka } from 'kafkajs';

async function producer() {
  const kafka = new Kafka({
    clientId: 'tester',
    brokers: ['machine-broker1:19092', 'machine-broker2:29092', 'machine-broker3:39092'],
  });

  const producer = kafka.producer();
  await producer.connect();
  
  await producer.send({
    topic: 'test-topic-1', 
    messages: [{key: 'any-key', value: 'any-value'}]
  })
  // }
}

producer().catch((e) => {
    throw e;
});

```

## Consumer Example
```js
import { Kafka, logLevel } from 'kafkajs';

async function consumer() {
  const kafka = new Kafka({
    clientId: 'tester',
    brokers: ['machine-broker1:19092', 'machine-broker2:29092', 'machine-broker3:39092'],
    logLevel: logLevel.ERROR,
  });
  
  const consumer = kafka.consumer({ groupId: 'tester' });
  await consumer.connect();
  await consumer.subscribe({ topic: 'test-topic-1', fromBeginning: true });

  await consumer.run({
    eachMessage: async ({ message }) => {
      console.log(JSON.stringify(message))
    },
  });
}

consumer().catch((e) => {
  throw e;
});

```

# akhq
You can access the GUI manager with your browser  
`127.0.0.1:8080`