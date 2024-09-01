# Lambda - S3 - DynamoDB

![image](https://github.com/user-attachments/assets/74f9ab94-d6a6-4c71-b42c-49036f2f8e1e)

## Practice

### 1. Resize images
- Create lambda function
```
const sharp = require("sharp")
const aws = require("aws-sdk")
const s3 = new aws.S3()

const width = parseInt(process.env.WIDTH);
const height = parseInt(process.env.HEIGHT);
const destinationBucket = process.env.DES_BUCKET;

exports.handler = async function(event, context){
    if (event.Records[0].eventName === "ObjectRemove:Delete"){
        return;
    }
    const BUCKET = event.Records[0].s3.bucket.name;
    const KEY = event.Records[0].s3.object.key;
    try {
        let image = await s3.getObject({ Bucket: BUCKET, Key: KEY }).promise();
        image = await sharp(image.Body)
        const resizeImage = await image.resize(width, height, {fit: sharp.fit.fill, withoutEnlargement: true}).toBuffer();
        await s3.putObject({ Bucket: destinationBucket, Body: resizeImage, Key: KEY }).promise();
        await s3.deleteObject({ Bucket: BUCKET, Key: KEY }).promise();
        return;
    } catch (err) {
        context.fail(`Error resizing image: ${err} `);
    }
}
```
- Create 2 bucket
- Set policy for lambda function
 

### 2. Post data to DynamoDB
Code Python 3.9

```
import boto3
import json

client = boto3.resource('dynamodb')

def lambda_handler(event, context):

    book_item = event["body"]
    error = None
    try:
        table = client.Table('Books')
        table.put_item(Item = book_item)
    except Exception as e:
        error = e

    if error is None:
        response = {
            'statusCode': 200,
            'body': 'writing to dynamoDB successfully!',
            'headers': {
                'Content-Type': 'application/json'
            },
        }
    else:
        response = {
            'statusCode': 400,
            'body': 'writing to dynamoDB fail!',
            'headers': {
                'Content-Type': 'application/json'
            },
        }

    return response

```

Data Test

```
{
"body": {
    "id": "1",
    "name": "Java",
    "author": "Alex",
    "category": "IT",
    "price": "10.89",
    "description": "This book guide to create Java web basic",
    "image": "https://book-image-resize-store.s3.us-east-1.amazonaws.com/Java.jpg"
}
}
```

