# ☁️ AWS Project - Create a basic Search Engine website using AWS

aws logo :aws:


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


```json
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


```python
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
        item_data = json.loads(json.dumps(item))  
        # Convert item to JSON-compatible format
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



## ➡️ Step 4 - Test Lambda Function

Let's test our newly created function. Testing this function will make sure we will receive gaming information back when we search for the game.

Get Information for Minecraft Game

1. Click the arrow on the "Test" button and click "Configure test events"



![Screenshot 2024-01-01 at 13 37 19](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/47223370-e946-41ad-a497-d7d008042c63)



2. Name the event "Test" then Paste the following JSON into the event. Afterwards, click "Save" to create.



```json
    {
       "Name of Game" : "Minecraft"
    }
```


![Screenshot 2024-01-01 at 13 38 19](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/a78c13c9-0d7f-4f5a-96df-275a09d39522)



3. Click "Test", and it will execute the test event. You should see the output in the console



![Screenshot 2024-01-01 at 13 42 46](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/14e855a2-fcc2-4194-a1b7-2617098f6131)




## ➡️ Step 5 - Create API and Deploy API

To create the API

1. Go to API Gateway console
2. Click "Create API"
3. Scroll down to REST API and click "Build"



![Screenshot 2024-01-01 at 13 44 16](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/9965d864-9361-45fd-a729-6d6ec8186cb2)



4. Make sure to select "New API" and Give the API name as GamingSearchEngine, keep everything as is, click "Create API"



![Screenshot 2024-01-01 at 13 45 17](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/13f8688b-67a7-4077-8687-6fa1d437329a)



5. Each API is collection of resources and methods that are integrated with backend HTTP endpoints, Lambda functions, or other AWS services. Typically, API resources are organized in a resource tree according to the application logic. At this time you only have the root resource, but let's add a resource next


Click on slash `/`, then click Create method"


![Screenshot 2024-01-01 at 13 47 15](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/1ac38b6b-df20-4f85-8394-c963d23c7063)



6. Select "POST" in the dropdown and click the check button

7. Keep the integration type as Lambda Function. Make sure the Region is the same as your Lambda Function and DynamoDB (It should be because of the default settings).

8. In Lambda Function text bar type in `GamingSearchEngine` and select that lambda function we created in the previous step. Click "Create method.


![Screenshot 2024-01-01 at 13 54 02](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/ec76497a-67d0-4bcd-97ac-ad7946e7fb00)



9. Click Enable CORS.



![Screenshot 2024-01-01 at 14 05 42](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/61786e30-a26a-431c-8e83-ce21f82f57ef)




10. Make sure `POST` is checked under "Methods" then click "Save"


![Screenshot 2024-01-01 at 14 06 50](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/384449f3-3c30-4302-af22-1800470154b9)



11. Click the "Deploy API" button. 


![Screenshot 2024-01-01 at 14 08 27](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/b1ad29aa-d79d-459d-b084-f96b2355bb41)



12. Click deployment stage, select "New Stage", Stage name `dev` then click "Deploy"



![Screenshot 2024-01-01 at 14 08 52](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/160c9f67-411b-40c9-827b-94b0741f8c2f)



13. Keep your Invoke URL handy. We will use this in the next step.


![Screenshot 2024-01-01 at 14 10 05](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/e40ab4fc-ea2f-4837-90b8-0b38717285b3)



## ➡️ Step 6 - Update the HTML Code and Save It

1. Copy the HTML code shown below


```html
   <html>
   <head>
    <title>Gaming Search Engine</title>
        <style>
            body {
            font-family: Arial, sans-serif;
            background-color: #f2f2f2;
            margin: 0;
            padding: 20px;
            }

            #container {
            max-width: 600px;
            margin: 0 auto;
            background-color: #fff;
            padding: 20px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
            }

            h1 {
            font-size: 32px;
            text-align: center;
            margin: 0;
            }

            form {
            text-align: center;
            margin-top: 40px;
            }

            input[type="text"] {
            width: 400px;
            height: 36px;
            padding: 8px;
            font-size: 16px;
            border: 1px solid #ccc;
            border-radius: 4px;
            }

            input[type="submit"] {
            background-color: #FFA500;
            border: none;
            padding: 8px 16px;
            font-size: 16px;
            cursor: pointer;
            }

            pre {
            margin-top: 40px;
            background-color: #f2f2f2;
            padding: 20px;
            white-space: pre-wrap;
            word-wrap: break-word;
            }
        </style>
    </head>
    <body>
    <div id="container">
        <h1>Gaming Search Engine</h1>
        <form id="apiForm" action="INVOKE URL" method="POST">
        <input type="text" id="nameInput" name="Name of Game" value="GameName">
        <br><br>
        <input type="submit" value="Submit">
        </form>
        <pre id="response"></pre>
    </div>

        <script>
            const form = document.getElementById('apiForm');
            const responsePre = document.getElementById('response');

            form.addEventListener('submit', function(event) {
            event.preventDefault();
            const formData = new FormData(form);

            fetch(form.action, {
                method: form.method,
                body: JSON.stringify(Object.fromEntries(formData)),
                headers: {
                'Content-Type': 'application/json'
                }
            })
            .then(response => response.json())
            .then(data => {
                responsePre.textContent = JSON.stringify(data, null, 2);
            })
            .catch(error => {
                responsePre.textContent = 'An error occurred: ' + error.message;
            });
            });
        </script>
    </body>
</html>


```



2. Paste it to Notepad (for windows), TextEdit (for apple), or a source code editor like Visual Studio Code (download the software from a Google Search)

3. Update the "INVOKE URL" in the code with your REST API Invoke URL from API Gateway in the previous step.

Insert your Invoke URL here. Keep the quotations around your link as well.


![Screenshot 2024-01-01 at 14 20 57](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/1e69b5f5-32ad-40f8-ab6b-aa21001c3335)



4. Save the file as `index` of html type. Thus, you may have to save it as `index.html`.
5. Turn the file into a "compressed (zipped) folder". Again, keep the name as `index`



## ➡️ Step 7 - Create Website using Amplify

1. Go to the AWS Amplify console
2. Click "GET STARTED" and then click get started for "Host your web app" under "Amplify Hosting"



![Screenshot 2024-01-01 at 14 18 20](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/4ef478bb-911a-4c4c-b523-cef52ec0d44b)



3. Click "Deploy without Git provider" option then click "Continue"


![Screenshot 2024-01-01 at 14 18 39](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/a743d2a8-8896-4248-9868-249860a51885)



4. Name the app `GamingSearchEngine`, Name the environment `dev`, then drag and drop the zipped `index` file that was saved in the previous step. Then click "Save and deploy".



![Screenshot 2024-01-01 at 14 22 28](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/cd3303ec-1474-4bb5-b05f-65fdf5e325be)



5. Once deployed, click the domain URL. You should be directed to the website we created via the HTML code.


![Screenshot 2024-01-01 at 14 24 36](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/e7d1dd96-8e8b-4bca-a420-8a0e84916d30)



Deployement Successufully Completed - 🌐 LIVE HTML Website


![Screenshot 2024-01-01 at 14 25 00](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/adedbed2-ae54-4c31-b40d-50d9e9724caa)



## ➡️ Step 8 - Test the Webpage


Type in "Minecraft" and you should receive a response back. Feel free to test the other game names as well (Must type the names of them exactly how they are in DynamoDB - Yes, Case Sensitive).


![Screenshot 2024-01-01 at 14 26 53](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/1b546a9e-4d57-4329-81ab-e4ad5f28781b)



## 💰 Cost

All services used are eligible for the AWS Free Tier. However, charges will incur at some point so it's recommended that you shut down resources after completing this tutorial.

## 🧹 Clean Up

🗑️  Delete the Web Hosting App - Amplify Console

🗑️  Delete the REST API - API Gateway Console

🗑️  Delete the Lambda Function - Lambda Console

🗑️  Delete the Database - DynamoDB Console






















