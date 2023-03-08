---
title: "AWS Services"
author: "Vladimir Lelicanin - SAE Institute"
format:
  revealjs:
    theme: default
    title-slide-attributes:
        data-background-image: https://keystoneacademic-res.cloudinary.com/image/upload/f_auto/q_auto/g_auto/w_256/element/15/156456_sae.jpg
        data-background-position: top left
        data-background-repeat: no-repeat
        data-background-size: 100px
    transition: fade
    footer: "Vladimir Lelicanin - SAE Institute Belgrade"
    margin: 0.2
    auto-animate: true
    preview-links: auto
    link-external-newwindow: true
    scrollable: true
    embed-resources: false
    slide-level: 1
    chalkboard: true
    multiplex: true
    slide-number: true
    incremental: false
    logo: https://upload.wikimedia.org/wikipedia/commons/e/e2/SAE_Institute_Black_Logo.jpg
---


## Introduction

We'll be discussing various Amazon Web Services that Laravel developers can use to enhance their applications.



## EC2 - Elastic Compute Cloud

EC2 is a scalable cloud computing-based service that provides developers with virtual servers to run their applications. Below is an example of how to create an EC2 instance in Laravel.
```php
$ec2Client = Aws\Ec2\Ec2Client::factory(array(
    'region' => 'us-west-2',
    'version' => 'latest',
));

$result = $ec2Client->runInstances(array(
    'ImageId' => 'ami-0323c3dd2da7fb37d',
    'MinCount' => 1,
    'MaxCount' => 1,
    'InstanceType' => 't2.micro',
));

$instanceId = $result->get('Instances')[0]['InstanceId'];
```

Documentation: <https://aws.amazon.com/ec2/>

---

## S3 - Simple Storage Service

S3 provides developers with virtually limitless storage space for their files and backups. It can also be used for static website hosting. Below is an example of how to upload a file to S3 in Laravel.
```php
use Illuminate\Support\Facades\Storage;

Storage::put('file.txt', 'Hello World');
```

Documentation: <https://aws.amazon.com/s3/>

---

## RDS - Relational Database Service

RDS provides managed relational databases for developers to use in their applications. It supports several popular databases, including MySQL and PostgreSQL. Below is an example of how to create an RDS instance in Laravel.
```php
$rdsClient = Aws\Rds\RdsClient::factory(array(
    'region' => 'us-west-2',
    'version' => 'latest',
));

$result = $rdsClient->createDBInstance(array(
    'DBInstanceIdentifier' => 'database-instance-1',
    'DBInstanceClass' => 'db.t2.micro',
    'Engine' => 'MySQL',
    'MasterUsername' => 'admin',
    'MasterUserPassword' => 'mysecretpassword',
));

$dbEndpoint = $result->get('DBInstance')->getEndpoint()->getAddress();
```

Documentation: <https://aws.amazon.com/rds/>

---

## Lambda

Lambda is a serverless computing service that allows developers to run code without managing servers. It can be used for background processing or to handle API requests. Below is an example of how to create a Lambda function in Laravel.
```php
$lambdaClient = Aws\Lambda\LambdaClient::factory(array(
    'region' => 'us-west-2',
    'version' => 'latest',
));

$result = $lambdaClient->createFunction(array(
    'FunctionName' => 'my-lambda-function',
    'Runtime' => 'nodejs12.x',
    'Role' => 'arn:aws:iam::123456789012:role/lambda-role',
    'Handler' => 'index.handler',
    'Code' => array(
        'ZipFile' => file_get_contents('lambda-function.zip'),
    ),
));
$functionArn = $result->get('FunctionArn');
```

Documentation: <https://aws.amazon.com/lambda>

---

## SQS - Simple Queue Service

SQS is a managed message queue service that allows developers to decouple and scale their applications. It can be used for background processing, distributed computing, and asynchronous tasks. Below is an example of how to send a message to SQS in Laravel.
```php
$sqsClient = Aws\Sqs\SqsClient::factory(array(
    'region' => 'us-west-2',
    'version' => 'latest',
));

$result = $sqsClient->sendMessage(array(
    'QueueUrl' => 'https://sqs.us-west-2.amazonaws.com/123456789012/my-queue',
    'MessageBody' => 'Hello World',
));
$messageId = $result->get('MessageId');
```

Documentation: <https://aws.amazon.com/sqs>

---

## CloudFront

CloudFront is a content delivery network that speeds up the delivery of your website or application's static and dynamic content. It reduces latency and improves performance for your users. Below is an example of how to create a CloudFront distribution in Laravel.
```php
$cloudFrontClient = Aws\CloudFront\CloudFrontClient::factory(array(
    'region' => 'us-west-2',
    'version' => 'latest',
));

$result = $cloudFrontClient->createDistribution(array(
    'DistributionConfig' => array(
        'CallerReference' => 'my-distribution',
        'Comment' => 'My Distribution',
        'DefaultRootObject' => 'index.html',
        'Enabled' => true,
        'Origins' => array(
            'Quantity' => 1,
            'Items' => array(
                array(
                    'Id' => 'my-bucket',
                    'DomainName' => 'my-bucket.s3.amazonaws.com',
                    'S3OriginConfig' => array(
                        'OriginAccessIdentity' => ''
                    ),
                ),
            ),
        ),
        'DefaultCacheBehavior' => array(
            'TargetOriginId' => 'my-bucket',
            'ForwardedValues' => array(
                'QueryString' => false,
            )
        )
    )
));
$distributionId = $result->get('Distribution')->getId();
```

Documentation: <https://console.aws.amazon.com/cloudfront/>

---

## Elastic Beanstalk

Elastic Beanstalk is a fully managed service that allows developers to quickly deploy and scale their applications. It supports several programming languages, including PHP. Below is an example of how to deploy a Laravel application to Elastic Beanstalk.
```yaml
# .elasticbeanstalk/config.yml
deploy:
  artifact: .elasticbeanstalk/eb.zip

container_commands:
  01-update-composer:
    command: "/usr/bin/composer.phar update"
    ignoreErrors: true
  02-generate-key:
    command: "copy .env.example .env && php artisan key:generate"
  03-run-migrations:
    command: "php artisan migrate --force"
  04-cache-clear:
    command: "php artisan cache:clear"
    ignoreErrors: true
  05-config-cache:
    command: "php artisan config:cache"
    leader_only: true
```

Documentation: <https://aws.amazon.com/elasticbeanstalk/>

---

## CloudWatch

CloudWatch is a monitoring service that provides developers with real-time insights and visibility into their applications, infrastructure, and services. Below is an example of how to create a CloudWatch alarm in Laravel.
```php
$cloudWatchClient = Aws\CloudWatch\CloudWatchClient::factory(array(
    'region' => 'us-west-2',
    'version' => 'latest',
));

$result = $cloudWatchClient->putMetricAlarm(array(
    'AlarmName' => 'my-alarm',
    'MetricName' => 'CPUUtilization',
    'Namespace' => 'AWS/EC2',
    'Statistic' => 'Average',
    'Period' => 60,
    'Threshold' => 70,
    'ComparisonOperator' => 'GreaterThanOrEqualToThreshold',
    'Dimensions' => array(
        array(
            'Name' => 'InstanceId',
            'Value' => 'i-0123456789abcdef0'
        ),
    ),
    'EvaluationPeriods' => 1,
    'AlarmActions' => array(
        'arn:aws:sns:us-west-2:123456789012:my-topic'
    ),
));
$alarmArn = $result->get('AlarmArn');
```

Documentation: <https://aws.amazon.com/cloudwatch/>

---

## SES - Simple Email Service

SES is an email service that allows developers to send and receive emails using their own domain names. It can be used for marketing campaigns and transactional emails. Below is an example of how to send an email using SES in Laravel.
```php
$sesClient = Aws\Ses\SesClient::factory(array(
    'region' => 'us-west-2',
    'version' => 'latest',
));

$result = $sesClient->sendEmail(array(
    'Source' => 'me@example.com',
    'Destination' => array(
        'ToAddresses' => array(
            'you@example.com'
        ),
    ),
    'Message' => array(
        'Subject' => array(
            'Data' => 'Hello',
        ),
        'Body' => array(
            'Text' => array(
                'Data' => 'Hello World',
            ),
        ),
    ),
));
$messageId = $result->get('MessageId');
```

Documentation: <https://aws.amazon.com/ses/>

---

## Secrets Manager

Secrets Manager is a secure and scalable service that allows developers to store their application secrets, passwords, and API keys in a centralized location. Below is an example of how to retrieve a secret value from Secrets Manager in Laravel.
```php
$secretsManagerClient = Aws\SecretsManager\SecretsManagerClient::factory(array(
    'region' => 'us-west-2',
    'version' => 'latest',
));

$result = $secretsManagerClient->getSecretValue(array(
    'SecretId' => 'my-secret',
));

$secretValue = $result->get('SecretString');
```

Documentation: <https://aws.amazon.com/secrets-manager/>

---

## API Gateway

API Gateway is a fully managed service that allows developers to create and manage APIs for their applications. It can be used for microservices and mobile back-end integrations. Below is an example of how to create an API Gateway and a resource in Laravel.
```php
$apiGatewayClient = Aws\ApiGateway\ApiGatewayClient::factory(array(
    'region' => 'us-west-2',
    'version' => 'latest',
));

$result = $apiGatewayClient->createRestApi(array(
    'name' => 'My API Gateway',
));

$apiId = $result->get('id');

$apiGatewayClient->createResource(array(
    'restApiId' => $apiId,
    'parentId' => NULL,
    'pathPart' => 'myresource',
));
```

Documentation: <https://aws.amazon.com/api-gateway/>

---

## Cognito

Cognito is a fully managed service that allows developers to create and manage user authentication and authorization for their applications. It can be used for web and mobile applications. Below is an example of how to authenticate a user using Cognito in Laravel.
```php
$cognitoClient = Aws\CognitoIdentityProvider\CognitoIdentityProviderClient::factory(array(
    'region' => 'us-west-2',
    'version' => 'latest',
));

$result = $cognitoClient->initiateAuth(array(
    'AuthFlow' => 'USER_PASSWORD_AUTH',
    'ClientId' => 'my-client-id',
    'AuthParameters' => array(
        'USERNAME' => 'my-username',
        'PASSWORD' => 'my-password',
    ),
));

$accessToken = $result->get('AuthenticationResult')->get('AccessToken');
```

Documentation: <https://aws.amazon.com/cognito/>

---

## DynamoDB

DynamoDB is a managed NoSQL database that provides fast and predictable performance. It can be used for real-time applications and mobile back-ends. Below is an example of how to create a DynamoDB table in Laravel.
```php
$dynamoDbClient = Aws\DynamoDb\DynamoDbClient::factory(array(
    'region' => 'us-west-2',
    'version' => 'latest',
));

$result = $dynamoDbClient->createTable(array(
    'TableName' => 'my-table',
    'AttributeDefinitions' => array(
        array(
            'AttributeName' => 'id',
            'AttributeType' => 'S',
        ),
    ),
    'KeySchema' => array(
        array(
            'AttributeName' => 'id',
            'KeyType' => 'HASH',
        ),
    ),
    'ProvisionedThroughput' => array(
        'ReadCapacityUnits' => 5,
        'WriteCapacityUnits' => 5,
    ),
));

$tableArn = $result->get('TableDescription')->get('TableArn');
```

Documentation: <https://aws.amazon.com/dynamodb/>

---

## Route 53

Route 53 is a scalable and highly available DNS service that allows developers to route traffic to their applications. It can be used for domain name registration and management. Below is an example of how to create a Route 53 record in Laravel.
```php
$route53Client = Aws\Route53\Route53Client::factory(array(
    'profile' => 'default',
    'region' => 'us-west-2',
    'version' => 'latest',
));

$result = $route53Client->changeResourceRecordSets(array(
    'HostedZoneId' => 'Z1234567890123',
    'ChangeBatch' => array(
        'Changes' => array(
            array(
                'Action' => 'CREATE',
                'ResourceRecordSet' => array(
                    'Name' => 'mydomain.com',
                    'Type' => 'A',
                    'TTL' => 300,
                    'ResourceRecords' => array(
                        array(
                            'Value' => '123.45.67.89',
                        ),
                    ),
                ),
            ),
        ),
    ),
));
```

Documentation: <https://aws.amazon.com/route53/>

---

## X-Ray

X-Ray is a debugging and profiling service that allows developers to identify and debug issues with their applications. It provides a detailed view of the requests and responses for each component of the application. Below is an example of how to use X-Ray in Laravel.
```php
$xrayClient = Aws\XRay\XRayClient::factory(array(
    'region' => 'us-west-2',
    'version' => 'latest',
));

$xrayClient->beginSegment(array(
    'Name' => 'my-segment',
));

// Do something here...

$xrayClient->endSegment(array(
    'EndTime' => microtime(true),
));
```

Documentation: <https://aws.amazon.com/xray/>

---

## Kinesis

Kinesis is a managed streaming service that allows developers to process and analyze real-time data from various sources. It can be used for log processing, click-stream analysis, and machine learning. Below is an example of how to create a Kinesis stream in Laravel.
```php
$kinesisClient = Aws\Kinesis\KinesisClient::factory(array(
    'region' => 'us-west-2',
    'version' => 'latest',
));

$result = $kinesisClient->createStream(array(
    'StreamName' => 'my-stream',
    'ShardCount' => 1,
));

$streamArn = $result->get('StreamDescription')->get('StreamARN');
```

Documentation: <https://aws.amazon.com/kinesis/>

---

## Step Functions

Step Functions is a serverless workflow service that allows developers to coordinate and orchestrate their applications. It can be used for long-running tasks and business processes. Below is an example of how to create a Step Function in Laravel.
```php
$stepFunctionsClient = Aws\StepFunctions\StepFunctionsClient::factory(array(
    'region' => 'us-west-2',
    'version' => 'latest',
));

$stateMachineArn = 'arn:aws:states:us-west-2:123456789012:stateMachine:my-state-machine';

$result = $stepFunctionsClient->startExecution(array(
    'stateMachineArn' => $stateMachineArn,
    'input' => '{"key": "value"}',
));

$executionArn = $result->get('executionArn');
```

Documentation: <https://aws.amazon.com/step-functions/>

---

## Athena

Athena is a serverless query service that allows developers to analyze data stored in S3 using SQL. It can be used for ad hoc queries and business intelligence. Below is an example of how to execute a query using Athena in Laravel.
```php
$athenaClient = Aws\Athena\AthenaClient::factory(array(
    'region' => 'us-west-2',
    'version' => 'latest',
));

$result = $athenaClient->startQueryExecution(array(
    'QueryExecutionContext' => array(
        'Database' => 'my-database',
    ),
    'QueryString' => 'SELECT * FROM my-table',
    'ResultConfiguration' => array(
        'OutputLocation' => 's3://my-bucket/results/',
    ),
));

$queryExecutionId = $result->get('QueryExecutionId');
```

Documentation: <https://aws.amazon.com/athena/>

---

## Conclusion

These are just a few of the AWS services that Laravel developers can use to enhance their applications. We encourage you to explore the full range of AWS services to find the ones that best fit your needs.

---

## References

- [AWS Docs](https://aws.amazon.com/documentation/)
- [Laravel Docs](https://laravel.com/docs)
- [AWS SDK for PHP Docs](https://docs.aws.amazon.com/sdk-for-php/v3/developer-guide/welcome.html)
- [Laravel AWS SDK Docs](https://github.com/aws/aws-sdk-php-laravel)
