
# 📚 Setting up Kafka Locally for Development and Testing (Simple Guide)

---

## 🚀 1. Prerequisites

Before we start:
- You need **Java Development Kit (JDK)** installed on your computer.  
  Kafka is built using Java, so it needs Java to work.  
  (Any version from Java 8 onwards is fine!)

---

## 📦 2. Download Apache Kafka

- Go to the [Apache Kafka website](https://kafka.apache.org/downloads).
- Download the latest **binary distribution** (something like `kafka_2.13-3.1.0.tgz`) depending on your computer.

---

## 🗜 3. Extract the Kafka Files

After downloading:
- Find the downloaded file.
- Extract (unzip) it into a folder.

If you are using a Mac/Linux, use this terminal command:
```bash
tar -xzf kafka_2.13-3.1.0.tgz
```

---

## 🛎 4. Start Zookeeper

Kafka needs a helper called **Zookeeper** to coordinate itself.

- Go into the Kafka folder in your terminal.
- Start Zookeeper by running:
```bash
bin/zookeeper-server-start.sh config/zookeeper.properties
```

---

## 🖥 5. Start the Kafka Broker

Now start the main Kafka server:
- Open a **new terminal window**.
- Go into the Kafka folder.
- Start Kafka broker:
```bash
bin/kafka-server-start.sh config/server.properties
```

---

## 🗂 6. Create a Topic

Now we need to create a "box" 📦 (called a **Topic**) where our messages will go.

Open another terminal and run:
```bash
bin/kafka-topics.sh --create --topic test-topic --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1
```

This creates a topic called `test-topic`.

---

## ✉️ 7. Produce and Consume Messages

Let’s now **send** and **read** messages.

- **To Send (Produce) Messages:**
```bash
bin/kafka-console-producer.sh --topic test-topic --bootstrap-server localhost:9092
```
(Type messages and press ENTER to send!)

- **To Read (Consume) Messages:**
```bash
bin/kafka-console-consumer.sh --topic test-topic --bootstrap-server localhost:9092 --from-beginning
```

You will see the messages you typed getting received!

---

## 🎯 8. Experiment and Explore

🎉 Congratulations!  
Now you have a working **Kafka** system on your laptop!

You can now:
- Create more topics
- Send more messages
- Try different settings like partitions, replication
- Try **groups** of consumers!

---

# 🛠 Hands-on Exercise: Installing Kafka and Building a Simple Producer-Consumer App

Now let's **actually build two small apps** —  
one **Producer** to send messages,  
one **Consumer** to receive messages.

---

## 🛠 Step 1: Install Kafka

Follow the same steps above:  
✅ Install JDK  
✅ Download Kafka  
✅ Extract Kafka files  
✅ Start Zookeeper and Kafka Broker

---

## 🛠 Step 2: Create a Topic

Let’s create another topic called `simple-topic`:
```bash
bin/kafka-topics.sh --create --topic simple-topic --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1
```

---

## ✍️ Step 3: Write the Producer App

In Java, create a file named `SimpleProducer.java`:

```java
import org.apache.kafka.clients.producer.*;
import java.util.Properties;

public class SimpleProducer {
    public static void main(String[] args) {
        Properties props = new Properties();
        props.put("bootstrap.servers", "localhost:9092");
        props.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer");
        props.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");

        Producer<String, String> producer = new KafkaProducer<>(props);

        try {
            for (int i = 0; i < 10; i++) {
                String message = "Message " + i;
                producer.send(new ProducerRecord<>("simple-topic", Integer.toString(i), message));
                System.out.println("Sent message: " + message);
                Thread.sleep(1000); // Pause for a second
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            producer.close();
        }
    }
}
```

This app **sends 10 messages** to the topic!

---

## ✍️ Step 4: Write the Consumer App

Create another file named `SimpleConsumer.java`:

```java
import org.apache.kafka.clients.consumer.*;
import org.apache.kafka.common.serialization.StringDeserializer;
import java.util.Collections;
import java.util.Properties;

public class SimpleConsumer {
    public static void main(String[] args) {
        Properties props = new Properties();
        props.put("bootstrap.servers", "localhost:9092");
        props.put("group.id", "test-group");
        props.put("key.deserializer", StringDeserializer.class.getName());
        props.put("value.deserializer", StringDeserializer.class.getName());

        Consumer<String, String> consumer = new KafkaConsumer<>(props);
        consumer.subscribe(Collections.singletonList("simple-topic"));

        try {
            while (true) {
                ConsumerRecords<String, String> records = consumer.poll(100);
                for (ConsumerRecord<String, String> record : records) {
                    System.out.println("Received message: " + record.value());
                }
            }
        } finally {
            consumer.close();
        }
    }
}
```

This app **listens** and **prints** every message it receives!

---

## ▶️ Step 5: Run Your Apps

- Start your **SimpleProducer**.
- Start your **SimpleConsumer**.

👉 You will see messages being sent and received live!

---

# 🎉 Conclusion

You have now:
- Installed Kafka locally
- Started Zookeeper and Kafka Broker
- Created Topics
- Sent and Received Messages
- Built your own **Producer-Consumer App**!

👏 Well done!  
Next step: **Tomorrow we will learn how to set up a full Kafka Cluster!**  
Stay tuned and happy messaging! ✨

