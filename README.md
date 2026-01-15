# TX-Processor Backend

Event-driven microservices API that orchestrates asynchronous file analysis by consuming SQS messages, reading files from S3, and persisting analysis results in Aurora.

This service is responsible for processing file analysis workflows and exposing REST endpoints to track processing status and results. It follows an event-driven architecture to ensure loose coupling, scalability, and reliable asynchronous execution across distributed components.

The API integrates with AWS-native services and is designed to handle background processing, state persistence, and result tracking in a consistent and resilient way.

## Table of Contents

- [TX-Processor](#tx-processor)
- [Tech stack](#tech-stack)
- [Business rules when validating](#business-rules-when-validating)
- [File processing statuses](#file-processing-statuses)


## Tech stack

![Java](https://img.shields.io/badge/Java-21-orange?style=flat&logo=openjdk)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.x-brightgreen?style=flat&logo=spring)
![AWS](https://img.shields.io/badge/AWS-Cloud-orange?style=flat&logo=amazonaws)
![AWS SQS](https://img.shields.io/badge/AWS_SQS-Async-blue?style=flat&logo=amazonaws)
![AWS S3](https://img.shields.io/badge/AWS_S3-Storage-blue?style=flat&logo=amazonaws)
![AWS Aurora](https://img.shields.io/badge/AWS_Aurora-Database-blue?style=flat&logo=amazonaws)
![Architecture](https://img.shields.io/badge/Architecture-Event--Driven-purple?style=flat)

## High-Level AWS Architecture

This diagram presents a high-level view of the cloud architecture, showing how clients interact with the system through an Angular frontend, API Gateway, and Application Load Balancer. The backend runs on Spring Boot services deployed on EC2 within an Auto Scaling Group, integrating with AWS services such as S3 for file storage, SQS for asynchronous processing, and Aurora for persistent data storage.


<img width="773" height="750" alt="image" src="https://github.com/user-attachments/assets/0085f596-fa0e-488d-a38a-d43a60540d4c" />

## Class Diagram (UML)

This diagram illustrates the main classes and their relationships within the application. It provides a high-level view of the domain model and highlights how core components interact during the file analysis and processing workflow.


<img width="948" height="311" alt="image" src="https://github.com/user-attachments/assets/6708986e-4f40-4dd0-ba8e-9b14cfd8daed" />


## Business rules when validating

- The input file must be a CSV file. Any other file format is rejected (e.g. TXT, MD, EXE).
- Each record in the CSV represents a transaction to be analyzed and validated.
- A transaction is considered valid only if all business validation rules are satisfied.
- The transaction_id must follow the expected format (e.g. TX-XXX).
- The transaction amount must be a positive numeric value.
- Transactions with zero or negative amounts are considered invalid.
- The currency field must contain a supported and valid currency code.
- The transaction date must be present and follow a valid date format.
- If at least one validation rule fails, the transaction is marked as invalid.
- The analysis result must persist both valid and invalid transactions for tracking purposes.
- Validation results are processed asynchronously as part of the event-driven workflow.

## File processing statuses

- pending: The process has been created and is waiting to be picked up for analysis.
- processing: The file is being processed.
- success: The processing workflow finished successfully from a tracking perspective.
- failed: The processing workflow failed due to an unexpected error during execution.
- rejected: The file or its content was rejected due to business validation rules.
