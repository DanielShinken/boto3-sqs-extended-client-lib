Boto3 SQS Extended Client Library for Python
===========================================

[![Build Status](https://travis-ci.org/timothymugayi/boto3-sqs-extended-client-lib.svg?branch=master)](https://travis-ci.org/timothymugayi/boto3-sqs-extended-client-lib)
[![codecov](https://codecov.io/gh/timothymugayi/boto3-sqs-extended-client-lib/branch/master/graph/badge.svg)](https://codecov.io/gh/timothymugayi/boto3-sqs-extended-client-lib)

The **Amazon SQS Extended Client Library for Python has been modelled after the original Amazon SQS Extended client library** This python library enables you to manage Amazon SQS message payloads with Amazon S3. This is especially useful for storing and retrieving messages with a message payload size greater than the current SQS limit of 256 KB, up to a maximum of 2 GB. Specifically, you can use this library to:

* Specify whether message payloads are always stored in Amazon S3 or only when a message's size exceeds 256 KB.

* Send a message that references a single message object stored in an Amazon S3 bucket.

* Get the corresponding message object from an Amazon S3 bucket.

* Delete the corresponding message object from an Amazon S3 bucket.

## Getting Started

* **Sign up for AWS** -- Before you begin, you need an AWS account. For more information about creating an AWS account and retrieving your AWS credentials, see [AWS Account and Credentials.
* **Sign up for Amazon SQS** -- Go to the Amazon [SQS console](https://console.aws.amazon.com/sqs/home?region=us-east-1) to sign up for the service.

* **Minimum requirements** -- To use the sample application, you'll need python 2.7 recommended to use 3 (and above)

* **Further information** - Read the [API documentation](http://aws.amazon.com/documentation/sqs/).


## Installation

Install using pip...

```pip install pysqs-extended-client```

## Usage

### Standard Queues

```
    import base64

    from pysqs_extended_client.SQSClientExtended import SQSClientExtended
    
    # from string import ascii_letters, digits
    # from random import choice
    
    from pysqs_extended_client.config import (AWS_SQS_QUEUE_URL, AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, AWS_DEFAULT_REGION)
    
    if __name__ == '__main__':
    
        sqs = SQSClientExtended(AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, AWS_DEFAULT_REGION, 'tiptapcode-sqs-data')
    
        # _100mb_large_string = ''.join([choice(ascii_letters + digits) for i in range(104857600)])
    
        # message = "_100mb_large_string"
    
        message = None
        with open("C:\\DjangoCourse\\Courses\\celery\\introduction-promo.mp4", "rb") as image_file:
            encoded_string = base64.b64encode(image_file.read())
            message = encoded_string.decode("utf-8")
    
        sqs.send_message(AWS_SQS_QUEUE_URL, message)
    
        res = sqs.receive_message(AWS_SQS_QUEUE_URL)
    
        for message in res:
            sqs.delete_message(AWS_SQS_QUEUE_URL, message.get('ReceiptHandle'))

```

### FIFO queues

```
    import base64
    import uuid

    from pysqs_extended_client.SQSClientExtended import SQSClientExtended
    
    # from string import ascii_letters, digits
    # from random import choice
    
    from pysqs_extended_client.config import (AWS_SQS_QUEUE_URL, AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, AWS_DEFAULT_REGION)
    
    if __name__ == '__main__':
    
        sqs = SQSClientExtended(AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, AWS_DEFAULT_REGION, 'tiptapcode-sqs-data')
    
        # _100mb_large_string = ''.join([choice(ascii_letters + digits) for i in range(104857600)])
    
        # message = "_100mb_large_string"
    
        message = None
        with open("C:\\DjangoCourse\\Courses\\celery\\introduction-promo.mp4", "rb") as image_file:
            encoded_string = base64.b64encode(image_file.read())
            message = encoded_string.decode("utf-8")

        # Create group and deduplication IDs, required for FIFO queues.
    		message_group_id = str(uuid.uuid4())
        message_deduplication_id = str(uuid.uuid4())

        sqs.send_message(AWS_SQS_QUEUE_URL, message, message_group_id=message_group_id, message_deduplication_id=message_deduplication_id)
    
        res = sqs.receive_message(AWS_SQS_QUEUE_URL)
    
        for message in res:
            sqs.delete_message(AWS_SQS_QUEUE_URL, message.get('ReceiptHandle'))

```


## Feedback

We use GitHub issues for tracking bugs and feature requests and have limited bandwidth to address them. Please use these community resources for getting help:

* Give feedback [here](https://github.com/timothymugayi/boto3-sqs-extended-client-lib/issues).
* If you'd like to contribute a new feature or bug fix, go ahead submit a pull request.