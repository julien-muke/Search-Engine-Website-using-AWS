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

