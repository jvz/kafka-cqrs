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
