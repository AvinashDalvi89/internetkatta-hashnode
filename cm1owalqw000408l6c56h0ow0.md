---
title: "How to Test ElastAlert Locally Using LocalStack: A Step-by-Step Guide"
seoTitle: "Test ElastAlert Locally with LocalStack - Guide"
seoDescription: "Guide to testing ElastAlert locally with LocalStack, covering setup, configurations, and troubleshooting to ensure a smooth alert migration process"
datePublished: Mon Sep 30 2024 10:57:03 GMT+0000 (Coordinated Universal Time)
cuid: cm1owalqw000408l6c56h0ow0
slug: how-to-test-elastalert-locally-using-localstack-a-step-by-step-guide
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1727693643075/80e25e7d-42e4-477d-a45c-a0acea0133ff.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1727693634226/9f3d64a9-7d28-4e27-ae2c-462cf29799d0.png
tags: aws, learning, elasticsearch, localstack, aws-community, elastalert

---

Hello Devs ,

I was recently assigned the task of migrating alert notifications from Slack to Microsoft Teams. However, I encountered challenges testing these alerts in a live environment, especially the inability to delete ECS clusters or tasks without impacting production systems. To address this issue, I looked for a way to simulate the alerting process locally. This search led me to discover LocalStack, a powerful tool that enables you to test AWS services on your local machine. In this blog, I will guide you through setting up LocalStack to test ElastAlert rules, ensuring a smooth alert migration without the risk of disrupting live services.

## 1\. Install Prerequisites

Before getting started, ensure you have the following installed:

* **Python**: Download and install Python (3.7 or above) from [python.org](https://www.python.org/downloads/).
    
* **AWS CLI**: Install the AWS CLI to manage LocalStack resources easily. Follow the [AWS CLI installation guide](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).
    
* **Docker**: Install Docker, as it is required for running LocalStack.
    

## 2\. Set Up LocalStack

### Using the LocalStack Desktop App

1. Download the **LocalStack Desktop app** from the [LocalStack website](https://localstack.cloud/).
    
2. Install the app by following the provided instructions for your operating system.
    
3. Open the LocalStack Desktop app and start the LocalStack service. This will create a LocalStack environment where you can run your AWS services locally.
    

### Using Docker Desktop LocalStack Extension

1. If you prefer using Docker, open **Docker Desktop**.
    
2. Navigate to the **Extensions** section and search for **LocalStack**.
    
3. Install the LocalStack extension directly from Docker Desktop.
    
4. Once installed, you can start LocalStack by selecting it from the extensions list.
    

## 3\. Create LocalStack Elasticsearch Instance

After installing LocalStack (either through the Desktop app or Docker extension), create an Elasticsearch domain. It may take a few minutes for the service to start.

Run the following command to create a LocalStack Elasticsearch domain: `—domain-name` can use any word I just used “testinglocally”

```plaintext
aws es create-elasticsearch-domain --domain-name testinglocally --endpoint=http://localhost:4566
```

Once you run this command it will show host value. copy that host value will require while doing step 4.

```json
{
    "DomainStatus": {
        "DomainId": "000000000000/testinglocally",
        "DomainName": "locales",
        "ARN": "arn:aws:es:us-east-2:000000000000:domain/testinglocally",
        "Created": true,
        "Deleted": false,
        "Endpoint": "testinglocally.us-east-2.es.localhost.localstack.cloud:4566",
        "Processing": true,
        "UpgradeProcessing": false,
        "ElasticsearchVersion": "7.10",
        "ElasticsearchClusterConfig": {
            "InstanceType": "m3.medium.elasticsearch",
            "InstanceCount": 1,
            "DedicatedMasterEnabled": true,
            "ZoneAwarenessEnabled": false,
            "DedicatedMasterType": "m3.medium.elasticsearch",
            "DedicatedMasterCount": 1
        }
    }
}
```

Verify the instance by listing the domains:

```plaintext
aws es describe-elasticsearch-domain --domain-name testinglocally --endpoint-url=http://localhost:4566
```

## 4\. Clone ElastAlert

Clone the ElastAlert repository from GitHub:

```plaintext
git clone https://github.com/jertel/elastalert2
cd elastalert2
```

### Create Configuration File

Copy the example configuration file and create a new `config.yaml`:

```plaintext
cp config.yaml.example config.yaml
```

You can place this file in the root directory or, if you prefer a structured approach, keep it in an `example` folder. If you do this, remember to specify the path when running ElastAlert rules.

## 5\. Install ElastAlert Dependencies

Install the required Python packages by running:

```plaintext
pip install -r requirements.txt
python setup.py install
```

## 6\. Create an ElastAlert Rule

Navigate to the `example/rules` folder in the ElastAlert repository. You can either select an existing example rule or create a new one for testing. Here’s an example structure for a new rule:

```yaml
name: "Sample Rule"
type: query
index: "filebeat-*"
filter:
  - query:
      query_string:
        query: 'monitor.status: "down"'
alert:
  - "email"  # Specify your alert method here
```

## 7\. Create an Index and Sample Data

Create the index that your rule will use, such as `heartbeat` or `filebeat`: You can use http://localhost:4566 or locales.us-east-2.es.localhost.localstack.cloud:4566.

```plaintext
curl -X PUT "http://localhost:4566/filebeat-1"
```

### Insert Sample Data

Use the following `PUT` request to add sample data:

Note : use UTC timing for testing if you want to know what is your machine UTC timing then run this command `date -u` it will display like this `Mon Sep 30 11:01:57 UTC 2024` take out of timing and use in below CURL request.

```powershell
curl -X PUT "http://localhost:4566/filebeat-1/_doc/1" -H 'Content-Type: application/json' -d '{
    "message": "Test log entry",
    "@timestamp": "2024-09-30T14:40:00Z",
    "monitor": {
        "name": "Test Monitor",
        "status": "down"
    }
}'
```

## 8\. Test the ElastAlert Rule

Run the ElastAlert test command:

```bash
elastalert-test-rule example/rules/example_error.yaml --alert
```

This command will simulate the alert based on the created rule and the data in your Elasticsearch instance.

## 9\. Troubleshooting and Triage

After running the test, you may want to perform some maintenance and triage tasks or while testing alerts locally, you may encounter some common errors. Here are some triaging steps you can follow:

### Deleting an Index

To delete an index, run: Whichever index you have created use that to delete example if you have created hearbeat-1 or filebeat-1 then used accordingly.

```bash
curl -X DELETE "http://localhost:4566/filebeat-1"
```

### Refreshing the Index

You can refresh the index to ensure that all operations are performed:

```bash
curl -X POST "http://localhost:4566/filebeat-1/_refresh"
```

### Searching the Index

You can search the index to verify data:

```bash
curl -X GET "http://localhost:4566/filebeat-1/_search"
```

### Searching with Filters

Apply filters to your search for more specific queries:

```bash
curl -X GET "http://localhost:4566/filebeat-1/_search?q=monitor.status:down"
```

### **Check Indexes**:

Run the following command to verify the indices in Elasticsearch

```bash
curl -X GET 'http://localhost:4566/_cat/indices?v'
```

### **Ensure Correct Timing**:

Alerts are often time-sensitive. Make sure the `@timestamp` field in your sample data falls within the range of the query in your rule. If your rule looks at data from a specific time range, ensure that your sample data aligns with this range.

### **Verbose Mode**:

If the alerts aren't triggering, run ElastAlert in verbose mode to get more detailed output\*\*.\*\* So that can see what going wrong. In case my case i created one index with filebeat and testing alert with heartbeat and this issue able to found because of verbose flag.

```bash
elastalert-test-rule examples/rules/example_error.yaml --alert --verbose
```

### **Error Resolution**:

Common errors like `index_not_found_exception` can be resolved by creating the index first or ensuring the correct configuration file paths.

## Conclusion

By following these steps, you can effectively test ElastAlert locally using LocalStack and simulate web hook alerts. This setup is ideal for development and testing environments where you can validate your alerting rules before deploying them to production.

Happy testing! 🚀

I hope this blog helps you learn. Feel free to reach out to me on my Twitter handle [@AvinashDalvi\_](https://hashnode.com/@AvinashDalvi_) leave a comment on the blog.

### References:

* [ElastAlert Documentation](https://elastalert.readthedocs.io/en/latest/index.html)
    
* LocalStack Documentation
    
* https://docs.localstack.cloud/user-guide/aws/elasticsearch/
    
* https://hub.docker.com/r/localstack/localstack#installing
    
* https://docs.aws.amazon.com/cli/latest/reference/es/describe-elasticsearch-domain.html