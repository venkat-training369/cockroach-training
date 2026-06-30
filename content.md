## CockroachDB DBA Course

## Module 1: Introduction to CockroachDB

* What is CockroachDB?
* Why Distributed SQL?
* SQL vs NoSQL vs Distributed SQL
* CockroachDB Features
* Use Cases
* Editions (Core, Enterprise, Cloud)
* Course Roadmap

---

## Module 2: Installation & Environment Setup

* Prerequisites
* Oracle Linux Installation
* Single-Node Cluster
* Secure vs Insecure Mode
* Single-Node Cluster (Production method)
  - CA Certificate Creation
  - Node Certificates
  - Client Certificates
  - TLS Authentication
* Web UI
* SQL Client
* Three-Node Cluster
* Cluster Verification
* Podman Desktop / Docker Installation


---

## Module 3: CockroachDB Architecture

* Distributed SQL Architecture
* Node & Cluster Architecture
* Store
* Key-Value(KV) Layer
* Ranges
* Replicas
* Leaseholder
* Gossip Protocol
* Raft Consensus
* Hands-On
     - Explore Internal metadata tables


---

## Module 4: SQL Fundamentals

* Databases
* Schemas
* Tables
* Data Types
* Constraints
* Primary Keys
* Secondary Indexes
* Sequences
* Views

---

## Module 5: Distributed Storage Internals

* Storage Engine
  - Pebble Storage Engine
  - SSTables
  - LSM Trees
  - Write Path
  - Read Path

* Range Architecture
  - Range Merging
  - Lease Transfers
  - Automatic Sharding
  - Rebalancing 

* Replication Internals
  - Replica Factor
  - Quorum
  - Failover Process
  - Leader Election

---
## Module 4: Database Administration

* Database Management
  - Create Databases
  - Create Schemas
  - Create Tables
  - Constraints
  - Sequences

* User Administration

 - Users
 - Roles
 - Grants
 - RBAC

* Multi-Tenancy

 - Logical Separation
 - Tenant Architecture
 - Resource Isolation

* Hands-On

- User Management Lab
- Security Configuration

333333333333333## Module 6: Transactions & MVCC

* ACID
* Serializable Isolation
* MVCC
* Transaction Lifecycle
* Read & Write Paths
* Timestamp Ordering
* Transaction Retries
* Savepoints

---

## Module 7: Performance Tuning

* EXPLAIN
* EXPLAIN ANALYZE
* Index Design
* Statistics
* Query Optimization
* Hotspots
* Vectorized Execution
* Session Settings

---

## Module 8: Cluster Administration

* User Management
* Roles
* Permissions
* Databases
* Schemas
* Cluster Settings
* Node Management
* Certificates

---

## Module 9: Backup & Restore

* Full Backup
* Incremental Backup
* Restore
* Scheduled Backups
* Cloud Storage Backups
* Disaster Recovery

---

## Module 10: Monitoring & Observability

* DB Console
* Metrics
* Prometheus
* Grafana
* Logs
* Health Checks
* Alerts
* Capacity Monitoring

---

## Module 11: High Availability & Replication

* Replication
* Quorum
* Leaseholder
* Node Failure
* Replica Recovery
* Rebalancing
* Zone Configuration
* Locality

---

## Module 12: Scaling CockroachDB

* Horizontal Scaling
* Adding Nodes
* Removing Nodes
* Automatic Rebalancing
* Capacity Planning
* Rolling Upgrades

---

## Module 13: Multi-Region Deployment

* Locality
* Regions
* Zones
* Survival Goals
* Geo-Partitioning
* Global Tables
* Latency Optimization

---

## Module 14: Security

* Certificates
* TLS
* Authentication
* Authorization
* Encryption at Rest
* Audit Logs
* Password Policies

---

## Module 15: Troubleshooting

* Node Down
* Disk Full
* High CPU
* High Latency
* Hot Ranges
* Leaseholder Imbalance
* Replica Issues
* Network Partition
* Slow Queries

---

## Module 16: Production Best Practices

* Cluster Sizing
* Hardware Recommendations
* Backup Strategy
* Upgrade Strategy
* Maintenance Windows
* Monitoring Checklist
* Security Checklist

---

## Module 17: CockroachDB Cloud

* Cluster Creation
* Scaling
* Backups
* Monitoring
* Security
* Connectivity

---

## Module 18: Migration to CockroachDB

* PostgreSQL to CockroachDB
* MySQL to CockroachDB
* Schema Migration
* Data Migration
* Validation

---

## Module 19: Hands-on Labs

* Install a Single-Node Cluster
* Build a Three-Node Cluster
* Create Databases & Tables
* Run Transactions
* Simulate a Node Failure
* Add a New Node
* Take a Backup & Restore
* Monitor Cluster Health

---

## Module 20: DBA Interview & Real-Time Scenarios

* Architecture Questions
* Replication Scenarios
* Performance Scenarios
* High Availability Questions
* Troubleshooting Scenarios
* Production Case Studies

T
