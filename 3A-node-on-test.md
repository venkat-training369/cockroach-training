**rework the entire process** because for a DBA course it should follow **PKI best practices** 

I **do not recommend** generating Node2's certificates on Node1 in your environment. Instead, let each node generate its own `node.crt` and `node.key` 
using the same CA. This is cleaner and scales naturally to Node3, Node4, etc.

---

### Step 7 - Generate Certificates (Node1)

## Environment

| Node  | Hostname | IP          | User      |
| ----- | -------- | ----------- | --------- |
| Node1 | oel9-n1  | 10.10.10.11 | cockroach |

Login as **venkat**

```bash
ssh venkat@10.10.10.11
```

Switch user

```bash
sudo su - cockroach
```

---

## Create CA Directory

```bash
mkdir -p /var/lib/cockroach/my-safe-directory
```

---

## Generate CA Certificate

```bash
cockroach cert create-ca \
    --certs-dir=/var/lib/cockroach/certs \
    --ca-key=/var/lib/cockroach/my-safe-directory/ca.key
```

This creates

```text
ca.crt
ca.key
```

Verify

```bash
ls -ltr /var/lib/cockroach/certs

ls -ltr /var/lib/cockroach/my-safe-directory
```

Expected

```text
/var/lib/cockroach/certs

ca.crt

----------------------------

/var/lib/cockroach/my-safe-directory

ca.key
```

---

## Generate Node1 Certificate

Still on **Node1**

```bash
cockroach cert create-node \
    localhost \
    127.0.0.1 \
    oel9-n1 \
    10.10.10.11 \
    192.168.235.247 \
    --certs-dir=/var/lib/cockroach/certs \
    --ca-key=/var/lib/cockroach/my-safe-directory/ca.key
```

Verify

```bash
ls -ltr /var/lib/cockroach/certs
```

Expected

```text
ca.crt

node.crt

node.key
```

---

## Generate Root Client Certificate

Still on Node1

```bash
cockroach cert create-client root \
    --certs-dir=/var/lib/cockroach/certs \
    --ca-key=/var/lib/cockroach/my-safe-directory/ca.key
```

Verify

```bash
ls -ltr /var/lib/cockroach/certs
```

Expected

```text
ca.crt

node.crt

node.key

client.root.crt

client.root.key
```

---

### Step 8 - Prepare Node2

Login

```text
Node2

Hostname : oel9-n2

User : venkat
```

Create directories

```bash
sudo mkdir -p /var/lib/cockroach/certs

sudo mkdir -p /var/lib/cockroach/my-safe-directory

sudo chown -R cockroach:cockroach /var/lib/cockroach
```

---

### Step 9 - Copy Only CA Files

**Login to Node1 as venkat**

Copy CA certificate

```bash
scp /var/lib/cockroach/certs/ca.crt venkat@10.10.10.12:/tmp/
```

Copy CA private key

```bash
scp /var/lib/cockroach/my-safe-directory/ca.key venkat@10.10.10.12:/tmp/
```

---

Login to Node2

Move files

```bash
sudo mv /tmp/ca.crt /var/lib/cockroach/certs/
```

```bash
sudo mv /tmp/ca.key /var/lib/cockroach/my-safe-directory/
```

Permissions

```bash
sudo chown cockroach:cockroach /var/lib/cockroach/certs/ca.crt

sudo chown cockroach:cockroach /var/lib/cockroach/my-safe-directory/ca.key
```

```bash
sudo chmod 644 /var/lib/cockroach/certs/ca.crt

sudo chmod 600 /var/lib/cockroach/my-safe-directory/ca.key
```

---

### Step 10 - Generate Node2 Certificate

Login

```text
Node2

User : cockroach
```

```bash
sudo su - cockroach
```

Generate Node2 certificate

```bash
cockroach cert create-node \
    localhost \
    127.0.0.1 \
    oel9-n2 \
    10.10.10.12 \
    192.168.235.248 \
    --certs-dir=/var/lib/cockroach/certs \
    --ca-key=/var/lib/cockroach/my-safe-directory/ca.key
```

Generate Root Client Certificate

```bash
cockroach cert create-client root \
    --certs-dir=/var/lib/cockroach/certs \
    --ca-key=/var/lib/cockroach/my-safe-directory/ca.key
```

Verify

```bash
ls -ltr /var/lib/cockroach/certs
```

Expected

```text
ca.crt

node.crt

node.key

client.root.crt

client.root.key
```

---

### Step 11 - Create Systemd Service

**Run on Node1 and Node2 as `venkat`**

```bash
sudo vi /etc/systemd/system/cockroach.service
```

### Node1

```ini
[Unit]
Description=CockroachDB Cluster
After=network.target

[Service]
Type=notify
User=cockroach
Group=cockroach

ExecStart=/usr/local/bin/cockroach start \
  --certs-dir=/var/lib/cockroach/certs \
  --store=/var/lib/cockroach/data \
  --listen-addr=10.10.10.11:26257 \
  --advertise-addr=10.10.10.11:26257 \
  --http-addr=0.0.0.0:8080 \
  --join=10.10.10.11:26257,10.10.10.12:26257 \
  --cluster-name=training-cluster

Restart=always
RestartSec=10

LimitNOFILE=1048576

[Install]
WantedBy=multi-user.target
```

### Node2

```ini
[Unit]
Description=CockroachDB Cluster
After=network.target

[Service]
Type=notify
User=cockroach
Group=cockroach

ExecStart=/usr/local/bin/cockroach start \
  --certs-dir=/var/lib/cockroach/certs \
  --store=/var/lib/cockroach/data \
  --listen-addr=10.10.10.12:26257 \
  --advertise-addr=10.10.10.12:26257 \
  --http-addr=0.0.0.0:8080 \
  --join=10.10.10.11:26257,10.10.10.12:26257 \
  --cluster-name=training-cluster

Restart=always
RestartSec=10

LimitNOFILE=1048576

[Install]
WantedBy=multi-user.target
```

---

### Step 12 - Start CockroachDB

**Run on both nodes as `venkat`**

```bash
sudo systemctl daemon-reload
```

```bash
sudo systemctl enable cockroach
```

```bash
sudo systemctl start cockroach
```

Verify

```bash
sudo systemctl status cockroach
```

---

### Step 13 - Initialize the Cluster

**Run only once on Node1 as `cockroach`**

```bash
cockroach init \
    --certs-dir=/var/lib/cockroach/certs \
    --host=10.10.10.11
```

Expected

```text
Cluster successfully initialized
```

---

## Why I recommend this approach

This workflow teaches an important PKI principle:

* One **CA** for the entire cluster.
* Each node has its **own** `node.crt` and `node.key`, generated locally.
* All nodes trust each other because they are signed by the **same CA**.
* When you add **Node3**, you simply repeat the same process on Node3—copy the CA, generate Node3's certificates locally, and join the cluster.

This is easier to understand and avoids the confusion of temporary certificate directories. One security note: in highly secure production environments, many organizations keep the `ca.key` offline on a dedicated CA host and generate node certificates there instead of copying the CA private key to every database node. For a training lab, copying `ca.key` to generate node certificates locally is acceptable and keeps the process straightforward.
