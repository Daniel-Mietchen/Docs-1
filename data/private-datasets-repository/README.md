# Datasets \(Preview\)

### Overview

Datasets are used to manage the flow of data with your machine learning workloads. Datasets have immutable versions that can be used to track your data as it changes. Dataset version can be used as input to Gradient workloads as well as outputs. Data is stored at a [Storage Provider](../storage-providers.md) and will be cached on a Gradient cluster's shared storage for a period of time so that data will be available readily on repeated usage.

{% hint style="info" %}
Gradient datasets require a private [Gradient Cluster](../../gradient-private-cloud/about.md).
{% endhint %}

### Versions, Tags, and Messages

Datasets have multiple versions that can be referenced. You can specify a message with a new dataset version to provide info around a newly created dataset version. In addition, you can tag a specific dataset version with a custom name as well. Here are the available ways to reference a dataset:

* \[dataset-id\]:latest, this will use the latest version of your dataset
* \[dataset-id\]:\[dataset-version\], this will the use the specified dataset-version
* \[dataset-id\]:\[dataset-tag\], this will use the specified  dataset version that the dataset-tag points to

### Committed state

Dataset versions have a uncommittted and committed state. When a Dataset is uncommitted, you can modify or add files freely. When a Dataset is committed it will be immutable \(will not allow any modifications\). This allows workloads to be repeatable and deterministic with the provided Datasets. 

## Creating a Dataset and Dataset Version

```text
$ gradient datasets versions create --id=dst364npcw6ccok --source-path=./some-data/
Created dataset version: dst364npcw6ccok:fo5rp4m
Committed dataset version: dst364npcw6ccok:fo5rp4m
```

## Using Datasets

### Jobs

You can use existing Datasets or create new ones with Gradient jobs. In the below scenario the following datasets actions are specified:

* **dst364npcw6ccok:fo5rp4m** will be mounted to: **/datasets/input-a**
* **dst364npcw6ccok:fo34ram** will be mounted to: **/datasets/input-b**
* A dataset will be mounted to: **/datasets/output-a** which will create dataset: **dst364npcw6ccok:latest**

```text
gradient jobs create \
  --clusterId=$CLUSTER \
  --machineType=$MACHINE_TYPE \
  --projectId=$PROJECT \
  --container=bash \
  --command='cat /datasets/input/hello.txt > /datasets/output/hello2.txt && date >> /datasets/output/hello2.txt' \
  --dataset=output-a@dst364npcw6ccok \
  --dataset=input-a@dst364npcw6ccok:fo5rp4m \
  --dataset=input-b@dst364npcw6ccok:fo34ram
```

## Viewing Datasets

```text
$ gradient datasets list
+------+-----------------+-------------------------+
| Name | ID              | Storage Provider        |
+------+-----------------+-------------------------+
| test | dst364npcw6ccok | test1 (splgct3arqdh77c) |
+------+-----------------+-------------------------+

$ gradient datasets details --id=dst364npcw6ccok
+-----------------+-------------------------+
| Name            | test                    |
+-----------------+-------------------------+
| ID              | dst364npcw6ccok         |
| Description     |                         |
| StorageProvider | test1 (splgct3arqdh77c) |
+-----------------+-------------------------+
```

## Viewing Dataset files

```text
$ gradient datasets files list --id=dst364npcw6ccok:fo5rp4m
+-----------+------+
| Name      | Size |
+-----------+------+
| hello.txt | 12   |
+-----------+------+
```

