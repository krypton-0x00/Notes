Is a open source distributed  event stream system, used in real time systems.
## Prerequisite

[](https://gist.github.com/piyushgarg-dev/32cadf6420c452b66a9a6d977ade0b01#prerequisite)

- Knowledge
    - Node.JS Intermediate level
    - Experience with designing distributed systems
- Tools
    - Node.js: [Download Node.JS](https://nodejs.org/en)
    - Docker: [Download Docker](https://www.docker.com/)
    - VsCode: [Download VSCode](https://code.visualstudio.com/)

## Commands

[](https://gist.github.com/piyushgarg-dev/32cadf6420c452b66a9a6d977ade0b01#commands)

- Start Zookeper Container and expose PORT `2181`.

```shell
docker run -p 2181:2181 zookeeper
```

- Start Kafka Container, expose PORT `9092` and setup ENV variables.

```shell
docker run -p 9092:9092 \
-e KAFKA_ZOOKEEPER_CONNECT=<PRIVATE_IP>:2181 \
-e KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://<PRIVATE_IP>:9092 \
-e KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR=1 \
confluentinc/cp-kafka
```

or Single command
```bash
docker run -p 9092:9092  -e KAFKA_ZOOKEEPER_CONNECT=172.19.144.1:2181 -e KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://172.19.144.1:9092 -e KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR=1  confluentic/cp-kafka
```

## Code

[](https://gist.github.com/piyushgarg-dev/32cadf6420c452b66a9a6d977ade0b01#code)

`client.js`

```js
const { Kafka } = require("kafkajs");

exports.kafka = new Kafka({
  clientId: "my-app",
  brokers: ["<PRIVATE_IP>:9092"],
});
```

`admin.js`

```js
const { kafka } = require("./client");

async function init() {
  const admin = kafka.admin();
  console.log("Admin connecting...");
  admin.connect();
  console.log("Adming Connection Success...");

  console.log("Creating Topic [rider-updates]");
  await admin.createTopics({
    topics: [
      {
        topic: "rider-updates",
        numPartitions: 2,
      },
    ],
  });
  console.log("Topic Created Success [rider-updates]");

  console.log("Disconnecting Admin..");
  await admin.disconnect();
}

init();
```

`producer.js`

```js
const { kafka } = require("./client");
const readline = require("readline");

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
});

async function init() {
  const producer = kafka.producer();

  console.log("Connecting Producer");
  await producer.connect();
  console.log("Producer Connected Successfully");

  rl.setPrompt("> ");
  rl.prompt();

  rl.on("line", async function (line) {
    const [riderName, location] = line.split(" ");
    await producer.send({
      topic: "rider-updates",
      messages: [
        {
          partition: location.toLowerCase() === "north" ? 0 : 1,
          key: "location-update",
          value: JSON.stringify({ name: riderName, location }),
        },
      ],
    });
  }).on("close", async () => {
    await producer.disconnect();
  });
}

init();
```

`consumer.js`

```js
const { kafka } = require("./client");
const group = process.argv[2];

async function init() {
  const consumer = kafka.consumer({ groupId: group });
  await consumer.connect();

  await consumer.subscribe({ topics: ["rider-updates"], fromBeginning: true });

  await consumer.run({
    eachMessage: async ({ topic, partition, message, heartbeat, pause }) => {
      console.log(
        `${group}: [${topic}]: PART:${partition}:`,
        message.value.toString()
      );
    },
  });
}

init();
```

## Running Locally

[](https://gist.github.com/piyushgarg-dev/32cadf6420c452b66a9a6d977ade0b01#running-locally)

- Run Multiple Consumers

```shell
node consumer.js <GROUP_NAME>
```

- Create Producer

```shell
node producer.js
```

```shell
> tony south
> tony north
```