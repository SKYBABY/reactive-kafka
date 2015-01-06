Reactive Streams for Kafka
====

[Reactive Streams](http://www.reactive-streams.org) wrapper for [Apache Kafka](https://kafka.apache.org/).

Supports Kafka 0.8.2-beta

Testing
----
Tests require Apache Kafka and Zookeeper to be available on localhost:9092 and localhost:2181

Example usage
----

```Scala
import akka.actor.ActorSystem
import akka.stream.FlowMaterializer
import akka.stream.scaladsl.{Sink, Source}
import com.softwaremill.react.kafka.ReactiveKafka

implicit val materializer = FlowMaterializer()
implicit  val actorSystem = ActorSystem("ReactiveKafka")

val kafka = new ReactiveKafka(host = "localhost:9092", zooKeeperHost = "localhost:2181")
val publisher = kafka.consume("lowercaseStrings", "groupName")
val subscriber = kafka.publish("uppercaseStrings", "groupName")


Source(publisher).map(_.toUpperCase).to(Sink(subscriber)).run()
```