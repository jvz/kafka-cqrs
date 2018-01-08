This project is provided as an example implementation of an event sourcing architecture using Apache Kafka.

## Event Sourcing Concepts

An *event* is an immutable data structure that carries information about something that happened inside an application in the past.
Events are generally named in a descriptive, past tense fashion.
For example, `UserCreated` may be an event name with an associated timestamp and other event details.

An *event log* is a linear sequence of events.

The concept of *event sourcing* involves modeling all data as events that are stored in an event log.
This is done instead of mutable CRUD operations.
The state of a system at any given time can be defined by the sequence of events leading up to that time.

The related concept of *CQRS*, or command query responsibility segregation, is an architectural pattern that separates the concerns of storing and retrieving data.
This can normally be implemented using an event log on the write side.
Such a pattern uses commands and queries to write and read data respectively.
Commands, once validated, generate events which can be subscribed to by other components in the system.
These events are used to calculate the state of an entity in the system by combining all relevant events.
The state of an entity can be periodically snapshotted to avoid having to reread the entire event log and thus avoid long reboot times as data expands.

## Apache Kafka

Apache Kafka is a distributed persistent messaging system.
A cluster of Kafka brokers is divided into topics, and each topic is divided into numerical partitions.
Within a topic, messages are identified by an offset.
Different partitioning strategies must be utilized to ensure closely related data are stored into the same partition.
This concern is also related to ordering guarantees for messages.

Our use of Kafka in an event sourced system will be as our write side data store along with using Kafka Streams or KSQL for creating read side views of the data.
In order to write or read data to a Kafka topic, we must define a pair of serializers and deserializers.
Common data formats for this include JSON and Apache Avro.
Combined with additional software from the Confluent Platform version of Apache Kafka, we can use a schema registry to manage use of Avro in a consistent fashion.
By using appropriate message retention policies, we can keep our event log indefinitely.
Additional topics can be created for representing snapshots of state which help provide a read side optimization.
For other more complex read side operations, we can subscribe to a topic using Kafka Streams or KSQL, aggregate data into local key/value stores (e.g., RocksDB), perform windowing, and other data enrichment.
This same pattern can be extended to copy written data into another data store such as Postgres using Kafka Connect to make our data queryable in a relational fashion.
