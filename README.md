# Heimdal | AI Threat Monitoring Platform
Heimdall is an artificial intelligence-based intrusion detection system designed to perform realtime monitoring and classification of network traffic using machine learning techniques. It captures live packets from the network, extracts relevant features, and streams this data through Apache Kafka, which handles high-throughput, asynchronous communication between system components. The Kafka consumer processes the incoming data and applies pre-trained machine learning models to classify packets as either benign or potentially malicious. Classification results and system metrics are displayed on a Flask-based web dashboard, offering users a clear view of ongoing traffic, detected threats, and system activity.

<img width="864" height="346" alt="System Architecture" src="https://github.com/user-attachments/assets/dcc6381f-1d5f-4de2-aa9d-b6c6834ecaaa" />

A central aspect of Heimdall is its adaptive learning mechanism, which enables the system to improve over time by retraining its models based on new labeled data. Retraining can be triggered automatically when a certain volume of labeled samples is collected or manually through the dashboard interface. This approach allows Heimdall to adjust to changes in network behavior and maintain relevant detection capabilities as traffic patterns evolve, addressing the problem of concept drift commonly found in static machine learning systems. Prometheus is used for monitoring and collecting performance metrics such as packet throughput, model accuracy, and retraining events, enhancing the system’s transparency and maintainability. The system is built using a modular architecture, separating key functions like data capture, classification, visualization, and retraining into independent components, which makes development and updates more manageable. Overall, Heimdall applies adaptive, datadriven learning to enhance the detection of network anomalies in environments where traffic  behavior is subject to frequent change.

<img width="829" height="149" alt="image" src="https://github.com/user-attachments/assets/2a851135-f2d4-4f3a-bff6-b42de69a9ce9" />

## Use Case Model
The Heimdall system consists of three main actor roles: the Network Administrator, the Security Analyst, and the System (Heimdall) itself. The use case model reflects interactions where the system captures real-time network traffic, analyzes it using ML classifiers, generates alerts, retrains models if necessary, and visualizes threat metrics through Prometheus and Grafana.

<p align="left">
<img width="725" height="460" alt="image" src="https://github.com/user-attachments/assets/ef405ad5-13b2-48f6-bcbd-2685c201209c" />
</p> 

<p align="left">
<img width="797" height="332" alt="image" src="https://github.com/user-attachments/assets/b7cdac7e-b32b-4b14-be1b-63aae9696f2e" />
</p> 

## Architecture Diagram

Heimdall follows a multi-tiered architecture consisting of four main layers: Presentation, Business Logic, Data Access, and Data Store. Each layer is responsible for specific tasks, enabling modular
development and easier maintenance.

• Tier 1 – Presentation Layer: This includes the UI Framework built with Flask and frontend technologies. It provides users with real-time dashboards to view logs, threat summaries, and retraining status.

• Tier 2 – Business Logic Layer: This tier handles core processing. It includes the Kafka Consumer Service (for feature ingestion), the Model Manager (for prediction logic), the
Metrics Collector (for Prometheus integration), and the Re-trainer Module (for model updates).

• Tier 2 – Data Access Layer: Interfaces between services and the data store. It includes the Kafka Producer (for feature publishing), Prometheus Exporter (for metrics exposure), and Logs Handler (for log persistence).

• Tier 3 – Data Store Layer: This tier holds persistent data such as the Kafka Topic stream, serialized ML model files (.pkl), and stored logs and system metrics. This layered approach ensures that components can be developed and tested independently,
improving system scalability and flexibility.

<p align="left">
<img width="783" height="531" alt="image" src="https://github.com/user-attachments/assets/4dc9d464-ab5c-4245-bf19-13eb9d7fb5f4" />
</p>



## Activity Diagram
The process begins with the activation of the Kafka producer, which captures network packets via Scapy and extracts relevant features. These are serialized into JSON and sent to a Kafka topic. The Kafka consumer retrieves these messages, preprocesses them, and uses preloaded ML models to predict whether the traffic is a threat. Based on the prediction, packets are logged as either benign or threats. Metrics are updated accordingly. If enough labeled data accumulates or manual retraining is triggered, the system initiates model retraining, updates the model's accuracy, and saves the new version. The dashboard component fetches the logs using Flask and displays the threat records to users, completing the cycle.
<img width="824" height="560" alt="image" src="https://github.com/user-attachments/assets/c1b9fb26-44a0-45dd-b574-ee41148cbe89" />

 ## Tools and Technologies 
 <img width="850" height="396" alt="image" src="https://github.com/user-attachments/assets/2b079154-1f2c-48df-bdde-85a7bbbb051e" />


## Data Flow diagram
The Data Flow Diagram (DFD) illustrates the overall flow of data within the Heimdall system. Itcaptures how network traffic is ingested, processed, and visualized by different components. The process begins with the capture of raw packets from the network, which are then streamed to a Kafka topic. These packets undergo feature extraction and are converted into structured feature vectors in JSON format. The consumer module retrieves and normalizes these vectors to perform threat classification. The results, including detected threats, are stored in the threat log and made available for visualization on the dashboard. Security analysts can manually initiate retraining if needed, and the system metrics are continuously monitored and visualized using Prometheus and Grafana. The diagram
also differentiates between three primary data types: raw traffic (blue), feature vectors (green), and logs/metrics (red), ensuring clear traceability of data movement across the system.

<img width="885" height="588" alt="image" src="https://github.com/user-attachments/assets/1ce5604d-8453-450e-93f9-10f021170b62" />

## Dashboard
![Dashboard - Threat Monitoring System](https://github.com/user-attachments/assets/e42bb717-f79b-4142-b0b8-3870c8765d7d)


Users interact with Heimdall through a lightweight web dashboard built with Flask and standard front-end technologies. The dashboard includes:

• A visual threat summary showing detected attack types and their frequency.

• A live log table displaying real-time classification results with timestamps and IP addresses.

• A retraining status section showing the number of labeled samples and retrain thresholds.

• A manual retraining button that allows users to trigger model updates.

• Simple navigation between core sections such as "Home" and "Logs." The dashboard is fully responsive and intended for desktop browsers.

