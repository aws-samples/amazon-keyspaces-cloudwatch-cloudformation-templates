# Amazon Keyspaces (for Apache Cassandra) CloudWatch CloudFormation Templates

This repository provides CloudFormation templates to quickly set up CloudWatch Metrics for Amazon Keyspaces. Using this template will allow you to get started more easily by providing deployable prebuilt CloudWatch dashboards with commonly observed metrics.

## Amazon Keyspaces (for Apache Cassandra)
[Amazon Keyspaces](https://aws.amazon.com/keyspaces/) is a scalable, highly available, and managed Apache Cassandra–compatible database service. Amazon Keyspaces is serverless, so you pay for only the resources you use. The service can automatically scale tables up and down in response to application traffic. There are no nodes, instances, or servers to manage. There are also no cassandra background processes to maintain such as compaction and repair. The service is designed to provide consistent low latencies at any scale.  

## Amazon Keyspaces & AWS CloudWatch
You can monitor Amazon Keyspaces using CloudWatch, which collects raw data and processes it into readable, near real-time metrics. These statistics are kept for 15 months, so that you can access historical information and gain a better perspective on how your web application or service is performing. To read more see the [Official Keyspaces Documentation](https://docs.aws.amazon.com/keyspaces/latest/devguide/monitoring-cloudwatch.html)


Following is an example of CloudWatch view of  
![CloudFormation Import](/static/images/cw_screenshot.png)


## AWS CloudFormation and Keyspaces
AWS CloudFormation provides a common language for you to model and provision AWS and third-party application resources in your cloud environment. AWS CloudFormation allows you to use programming languages or a simple text file to model and provision, in an automated and secure manner, all the resources needed for your applications across all regions and accounts. This gives you a single source of truth for your AWS and third-party resources.

You can use AWS CloudFormation to create and manage settings for Amazon Keyspaces resources based on templates. The templates enable you to specify the name of keyspaces and tables. You can also specify the schema, read/write mode, and provisioned throughput settings for tables.

For more information, see the [Cassandra Resource Type Reference](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/AWS_Cassandra.html) in the [AWS CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html).

## Deploy using the AWS CloudFormation console
* Clone this repository or download the template file.
* Navigate to [CloudFormation Home](https://console.aws.amazon.com/cloudformation/home) and Click _Create Stack_
* Import the CloudFormation Template
* Enter the parameters and go!

#### Navigate to CloudFormation
![CloudFormation Home](/static/images/cf_home_screenshot.png)

#### Import Template
![CloudFormation Import](/static/images/cf_import_screenshot.png)

#### Configure and go!
![CloudFormation Import](/static/images/cf_create_cw_for_table.png)

# Templates
### keyspaces-table-cw.yaml
This cloud formation template will setup a new cloud watch dashboard containing metrics for a Amazon Keyspaces' table of your choice. You must provide a valid keypsace name, table name and region. In seconds you will have a new dashboard in CloudWatch containing metrics for the provided table.

Some of the metrics include
* Consumed and Provisioned Capacity per second
* Number of CQL Request per second
* Average Latency per Second
* User errors
* System Errors
* Current Account Quotas

## Executing from the AWS CLI
In this example we create a new CloudWatch dashboard containing metrics for a Amazon Keyspaces table. The following code snippet uses the AWS CLI to execute CloudFormation template from this GitHub repository. The script will pipe the contents of keyspaces-table-cw.yaml into an environment variable which we then pass into the `--template-body` parameter. Stack name and cloudwatch dashboard will be generated for you, but you must provide the following parameters for the below script to work.

In the script below, replace the following paramters with your table information
* `keyspace_name` The name of your table's keyspace
* `table_name` The name of your table
* `region_name` The name of the region where your table resides

```sh
#!/bin/bash

git clone https://github.com/aws-samples/amazon-keyspaces-cloudwatch-cloudformation-templates.git

cd amazon-keyspaces-cloudwatch-cloudformation-templates

#your table info here
export keyspace_name=my_keyspace
export table_name=my_table
export region_name=us-east-1

export mytemplate=$(cat keyspaces-table-cw.yaml)
export dashboard_name="keyspaces-$( echo ${keyspace_name}--${table_name}-$$ | tr '_' '-' )"
export stack_name="cw-${dashboard_name}"

aws cloudformation create-stack --stack-name "${stack_name}" --parameters ParameterKey=CloudwatchDashBoardNameParameter,ParameterValue=${dashboard_name} ParameterKey=CassandraKeyspaceParameter,ParameterValue=${keyspace_name} ParameterKey=CassandraTableParameter,ParameterValue=${table_name} ParameterKey=CassandraTableRegion,ParameterValue=${region_name} --template-body $mytemplate

```

# Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

# License

This library is licensed under the MIT-0 License. See the LICENSE file.
