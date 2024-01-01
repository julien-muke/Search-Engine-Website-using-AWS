# ☁️ AWS Project - Create a basic Search Engine website using AWS

## 📄 Introduction

In this lab we will create a Gaming Search Engine (with a database of 5 games) from scratch in the AWS console using services/components like DynamoDB, Amplify, and API Gateway. Our Website will be hosted on a static HTML website. The HTML website will be powered by Amplify and will function from the API we create in API Gateway.


## 📐 Architecture Design


As shown on the architecture diagram below: the users is going to log on to the AWS amplify and Amplify is going to hold our static website this is actually going to be the user interface and input a seach, once we search it's going to call on Amazon API Gateway, then it's going to call on AWS Lambda which is going to ask for permission in order to access Amazon DynamoDB which has the information that we're searching for in the AWS Amplify static website.

![AWS diagram-6](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/1ceb6f26-78b9-4e23-ba9a-ffa088b395b1)



## ➡️ Step 1 - Create DynamoDB table

👉 Create DynamoDB table

1. Go to the DynamoDB console
2. Click "Create Table"


![Screenshot 2024-01-01 at 12 34 38](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/8eacb8e8-ca9a-4efd-8ba9-7832d3eebf46)



3. Name the table "GamingSearchEngine" and name the partition key as "Name of Game". Keep everything else default


![Create-table-Amazon-DynamoDB-Management-Console-DynamoDB-us-east-1 (1)](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/6f6f4acc-e31e-49b6-8055-0f2afaaaa125)



4. Click Create table at the bottom of the screen (may take a few minutes to create)



👉 Populate DynamoDB table

Click the check box for the newly created database, click "Actions" dropdown menu, then click "Explore items"


![Screenshot 2024-01-01 at 12 44 05](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/14fe9fa7-cc1a-422c-8a8b-f9d40de92760)



2. Click "Create item" button
3. Fill out the Attribute value as shown in the screenshot below. To add the Category and ESRB Rating Attribute, click "Add new attribute" and select "String"



![Screenshot 2024-01-01 at 12 47 23](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/294dbdfa-58e0-40cc-bb38-dd440573300b)




4. Repeat step for all 5 games shown below


| Name of Game | Category | ESRB Rating |
|--------------|----------|------------|
| Grand Theft Auto V | Open World - Shooter | Mature - 17+ |
| Fortnite | Battle Royal Game | Teen - 13+ |
| God of War | Action-Adventure | Mature - 17+ |
| Call of Duty Warzone | First-Person Shooter | Mature - 17+ |
| Minecraft | Survival Game | Everyone - 10+ |



Note: You can duplicate an item to help expedite the process - shown in screenshot below 


![Screenshot 2024-01-01 at 12 48 17](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/a490ee55-aca0-4dad-b6db-c7885787d927)



5. Once complete, the items in your table should look like what's shown below



![Screenshot 2024-01-01 at 12 53 58](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/f2608c8a-ca46-4bd7-bde9-ace52bf546df)



## ➡️ Step 2 - Create Lambda IAM Role

Create the execution role that gives your function permission to access AWS resources.

To create an execution role

1. Open the roles page in the IAM console.
2. Choose Create role.
3. Create a role with the following properties.
        * Trusted entity – Lambda.
        * Role name – lambda-apigateway-dynamodb-role.
        * Permissions – Custom policy with permission to DynamoDB and CloudWatch Logs. This custom policy has the permissions that the function needs to write data to DynamoDB and upload logs.


![Screenshot 2024-01-01 at 12 55 45](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/dc60c1c1-23bb-4fe0-8882-db00d51654db)



![Create-policy-IAM-Global (3)](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/1fafa468-72e9-42f3-a819-f83f4d16743b)


```bash
{
"Version": "2012-10-17",
"Statement": [
    {
        "Sid": "VisualEditor0",
        "Effect": "Allow",
        "Action": [
            "logs:CreateLogStream",
            "dynamodb:PutItem",
            "dynamodb:DeleteItem",
            "dynamodb:GetItem",
            "dynamodb:Scan",
            "dynamodb:Query",
            "dynamodb:UpdateItem",
            "logs:PutLogEvents",
            "logs:CreateLogGroup"
        ],
        "Resource": "*"
    }
]
}
```



![Create-policy-IAM-Global (4)](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/c2df491e-81b5-49a4-9c56-829cbaa12c7d)




![Screenshot 2024-01-01 at 13 00 34](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/ca7fec1e-ed53-4be7-9cea-dae928c0d283)



![Screenshot 2024-01-01 at 13 01 29](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/3bb32d38-8ef1-47c2-ac34-7136c129c9df)




![Screenshot 2024-01-01 at 13 10 44](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/e6ab0780-fc52-470a-9789-a25d1510cc39)



![Create-role-IAM-Global (3)](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/3cfd440d-b678-4d11-b3af-bf0ac3d893ec)



## ➡️ Step 3 - Create Lambda Function

To create the function

1. Go to the Lambda console
2. Click "Create function" in AWS Lambda Console



![Screenshot 2024-01-01 at 13 33 04](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/3d83c8da-6192-40ee-87e9-6e4f5f252c9b)



3. Select "Author from scratch". Use name GamingSearchEngine , select Python 3.8 as Runtime. Under Permissions, select "Use an existing role", and select lambda-apigateway-dynamodb-role that we created, from the drop down

4. Click "Create function"



![Create-function-Lambda (1)](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/dd0fc78d-98f1-4aca-a113-508ca3c3284f)




5. Replace the existing sample code with the following code snippet and click "Deploy"


👉 Python CRUD Operation Code


```bash
    import boto3
    import json

    def lambda_handler(event, context):
    # Create a DynamoDB client
    dynamodb = boto3.resource('dynamodb')

    # Retrieve the partition key value from the event
    partition_key_value = event['Name of Game']

    # Get the DynamoDB table
    table = dynamodb.Table('GamingSearchEngine')

    # Call the get_item operation
    response = table.get_item(
        Key={
            'Name of Game': partition_key_value
        }
    )

    # Process the response
    if 'Item' in response:
        item = response['Item']
        # Do something with the item
        item_data = json.loads(json.dumps(item))  # Convert item to JSON-compatible format
        return {
            'statusCode': 200,
            'body': item_data
        }
    else:
        return {
            'statusCode': 404,
            'body': 'Item not found'
        }

        
```



![Screenshot 2024-01-01 at 13 36 30](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/a6436c90-e183-4c28-b355-2c7fb74a7604)



