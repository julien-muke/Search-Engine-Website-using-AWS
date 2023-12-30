# ☁️ AWS Project - Create a basic Search Engine website using AWS

## 📄 Introduction

In this lab we will create a Gaming Search Engine (with a database of 5 games) from scratch in the AWS console using services/components like DynamoDB, Amplify, and API Gateway. Our Website will be hosted on a static HTML website. The HTML website will be powered by Amplify and will function from the API we create in API Gateway.


## 📐 Architecture Design


As shown on the architecture diagram below: the users is going to log on to the AWS amplify and Amplify is going to hold our static website this is actually going to be the user interface and input a seach, once we search it's going to call on Amazon API Gateway, then it's going to call on AWS Lambda which is going to ask for permission in order to access Amazon DynamoDB which has the information that we're searching for in the AWS Amplify static website.

![AWS diagram-6](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/1ceb6f26-78b9-4e23-ba9a-ffa088b395b1)


