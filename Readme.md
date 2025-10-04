### Data Lakehouse & Search Indexing Pipeline System ###

This project demonstrates a modern data engineering architecture that integrates multiple data sources, batch and streaming ETL pipelines, Delta Lake storage, denormalization layers, and near-real-time indexing into OpenSearch.<br>

The system is designed to handle content data (posts, jobs, events, publications) and user profile data efficiently, providing a foundation for analytics dashboards, search, and recommendation engines.

### Architecture Overview ###

* Data Ingestion

Event Data (Posts, Jobs, Events, Publications):<br>
Captured in EventStoreDB.

User Profile Data & Metadata:<br>
Stored in PostgreSQL databases.

* ETL to Delta Lake

- Batch ETL Pipelines (Event Data)<br>
Each content type has its own batch ETL job, which reads from EventStoreDB and writes to Delta Lake:

Post-DeltaLake-Batch-ETL<br>
Explore the complete implementation and source code:<br>
https://github.com/Ayham-0010/Post-Deltalake-Batch-ETL<br>

Similar logic is repeated for:

Jobs-DeltaLake-Batch-ETL<br>
Events-DeltaLake-Batch-ETL<br>
Publications-DeltaLake-Batch-ETL

EventStoreDB client → Event-driven batch processing → Delta Lake

- Streaming ETL Pipeline (User Data)

User-DeltaLake-Stream-ETL<br>
Explore the complete implementation and source code:<br>
https://github.com/Ayham-0010/User-DeltaLake-Stream-ETL

Debezium Kafka Connector captures CDC from PostgreSQL.<br>
Changes are written to Kafka topics.<br>
Spark Structured Streaming consumes Kafka data, transforms it, and writes into Delta Lake.

- Denormalization & Indexing<br>
Each entity type has its own denormalization pipeline:

User-Denormalization-Pipeline<br>
Explore the complete implementation and source code:<br>
https://github.com/Ayham-0010/User-Denormalization-Pipeline

Similar logic is repeated for:<br>
Post-Denormalization-Pipeline<br>
Jobs-Denormalization-Pipeline<br>
Events-Denormalization-Pipeline<br>
Publications-Denormalization-Pipeline

* Optimizations:

Caching for performance and reduced shuffle<br>
leveraging python multi-threading to manage concurrency and avoid blocking<br>
Broadcast joins when applicable<br>
Uses Delta Lake CDC to capture updates and maintained consistency


* Sink:

Denormalized data → Kafka topics<br>
Kafka OpenSearch Connector → Indexes into respective OpenSearch indices


### Architecture Diagram ###

Here’s a clear visual of the end-to-end pipeline System:<br>
[Data Lakehouse & Search Indexing Pipeline Diagram]([URL](https://github.com/Ayham-0010/Data-Engineering-Portfolio/blob/main/%20Data%20Lakehouse%20%26%20Search%20Indexing%20Pipeline%20Diagram.png))



### Tech Stack ###

Databases: EventStoreDB, PostgreSQL<br>
Data Lake: Delta Lake (on top of Apache Spark)<br>
Streaming: Apache Kafka, Debezium, Spark Structured Streaming<br>
Search & Indexing: OpenSearch + Kafka Connect<br>
ETL Orchestration: Spark Batch & Streaming Jobs



### Future Enhancements ###

Add monitoring with Prometheus + Grafana<br>
Integrate Airflow for pipeline orchestration<br>
Add real-time dashboards powered by OpenSearch
