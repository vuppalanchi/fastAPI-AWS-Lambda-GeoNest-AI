# fastAPI-AWS-Lambda-GeoNest-AI [ FastAPI App Deployment Using AWS Lambda And API Gateway / AWS Function ]
# Architect : Upender Vuppalanchi
# Used for product GeoNest-AI for AI/ML Modelling [ training the AI Model ]

FastAPI is a modern fast (high-performance) web framework for building APIs with Python.

FastAPI features
FastAPI gives you the following, based on open standards

OpenAPI for API creation, including declarations of path operations, parameters, body requests, security, etc.
Automatic data model documentation with JSON Schema (as OpenAPI itself is based on JSON Schema).
Allows using automatic client code generation in many languages.
In this blog we are going to learn how to deploy containerised FastAPI Application using AWS Lambda and API Gateway.

FastAPI in Containers -Docker :

When deploying FastAPI applications, a common approach it is to build a Linux container image. It’s normally done using Docker. Then you can deploy that container image in Lambda . There are several advantages of using Linux containers, including security, replicability, simplicity, and others.

Architecture :
As per below Architecture , end users can able to access the FastAPI api endpoint either using Lambda URL or using API Gateway endpoint.

<img width="771" height="251" alt="image" src="https://github.com/user-attachments/assets/5917b9b4-2951-4898-8ed9-216c3fefa4b5" />

Prerequisites:
Before we start deploying a FastAPI docker container app, we need to install some softwares locally to build the app container image and push to ECR.

Install below tools in your local machine before we start to deploy FastAPI container app

Install Python
Install Docker
Install and configure the AWS CLI
Create ECR Repository in your AWS Account

Steps:
Build FastAPI container image in the local machine
Push the container image to ECR repo
Create a lambda function using ECR image
Test the lambda function
Create API Gateway
Integrate API Gateway with Lambda Function
Test the FastAPI app
Create Lambda URL and Test the FastAPI app


1.Build FastAPI container image in the local machine
Here, I used a simple FastAPI application that will display plain text.

Below is our repo structure

<img width="260" height="171" alt="image" src="https://github.com/user-attachments/assets/03dd4713-8cfd-4481-af1d-9ec2c894fbfd" />


Create the FastAPI Code:

Create an app directory and enter it.
Create an empty file __init__.py.
Create an app.py file with the python code
app.py :

refre app.py source code


Mangum allows us to wrap the API with a handler that we will package and deploy as a Lambda function in AWS. Then using AWS API Gateway we will route all incoming requests to invoke the lambda and handle the routing internally within our application.

FastAPI does not contain any built-in development server. Hence we need Uvicorn. It implements ASGI standards and is lightning fast.

uvicorn is an ASGI (async server gateway interface) compatible web server. It is (simplified) the binding element that handles the web connections from the browser or api client and then allows FastAPI to serve the actual request.

Create Dockerfile :

Create a Dockerfile inside the project directory

Dockerfile:

refer docker file source code

Create requirements.txt file :

You would normally have the package requirements for your application in some file.

It would depend mainly on the tool you use to install those requirements.

The most common way to do it is to have a file requirements.txt with the package names and their versions.

refer requirements.txt in the source code


Push the container image to ECR repo
Connect to ECR Repo from your local machine.
Note: Make sure you are able to connect aws and access the resources before starting to push the container image.


<img width="1096" height="32" alt="image" src="https://github.com/user-attachments/assets/3f75c3c2-77a5-43c6-9112-fb5701fdb18c" />


Build  the container image to ECR repo


<img width="1165" height="437" alt="image" src="https://github.com/user-attachments/assets/b22815a8-46db-4040-99d3-7a3131f1e9d8" />



Push the container image to ECR repo

<img width="1051" height="197" alt="image" src="https://github.com/user-attachments/assets/a624033f-e4b9-4e3d-94e6-b1e288f69377" />


Create a lambda function using ECR image:
Creating the Lambda function with container image.
Go to the lambda resource console and click on create function and select container image and Browse the image from ECR repo.

<img width="1205" height="611" alt="image" src="https://github.com/user-attachments/assets/cc539ac4-27bb-4e10-901d-2ca2e217f9cc" />


Test the Lambda Function:
To Test the lambda function select the Template “apigateway-aws-proxy” and edit the JSON configuration
Update the below mentioned details in the configuration and run the Test. function should be executed without any errors.


 <img width="702" height="153" alt="image" src="https://github.com/user-attachments/assets/5681e79e-78a4-4f2a-85f0-e1df2b9dfe20" />


 Note : These need to be updated according to the requirement of API calls


Create API Gateway :
Create the API Gateway - REST API.
Go to the API Gateway console and click on Create API and then select REST API Build.


<img width="1400" height="775" alt="image" src="https://github.com/user-attachments/assets/ef841c6d-ac39-49a9-9945-61500f867630" />


Integrate API Gateway with Lambda Function :
Create a Resource for API.
Create a Method for the Resource.



<img width="1400" height="651" alt="image" src="https://github.com/user-attachments/assets/e238d7c5-2b16-4642-860a-c62d93a6ff87" />



Once Resources and Method are created for API, then click on actions and select Deploy API and provide the required details like below.
Press enter or click to view image in full size


<img width="1190" height="782" alt="image" src="https://github.com/user-attachments/assets/94aed571-b7b5-4a4a-8b50-40dbd53ffa6b" />


<img width="1400" height="329" alt="image" src="https://github.com/user-attachments/assets/5a55e15d-426a-4176-98ff-24fb0dfa7df5" />


Test the FastAPI app :
Take the URL of the Deployment of API Gateway and hit it in the browser and you will get the JSON response from FastAPI app.
Press enter or click to view image in full size


<img width="873" height="183" alt="image" src="https://github.com/user-attachments/assets/273ebb65-d843-4c38-98e5-894c96cdd52c" />

Create Lambda URL and Test the FastAPI app :
A function URL is a dedicated HTTP(S) endpoint for your Lambda function. You can create and configure a function URL through the Lambda console or the Lambda API. When you create a function URL, Lambda automatically generates a unique URL endpoint for you. Once you create a function URL, its URL endpoint never changes. Function URL endpoints have the following format: https://<url-id>.lambda-url.<region>.on.aws
Function URLs are dual stack-enabled, supporting IPv4 and IPv6. After you configure a function URL for your function, you can invoke your function through its HTTP(S) endpoint via a web browser, curl, Postman, or any HTTP client.


<img width="1400" height="314" alt="image" src="https://github.com/user-attachments/assets/21788413-9963-4d02-98c1-b42767cb5b32" />

Create Lambda URL and Test the FastAPI app :
While creating the Function URL choose the options auth type as NONE and CORS , leave remaining options as default values.
Save the configuration.


<img width="504" height="1076" alt="image" src="https://github.com/user-attachments/assets/46056238-369f-4df6-be8a-5d9c128d2da7" />


If your function URL uses the NONE auth type, you don’t have to sign your requests using SigV4. You can invoke your function using a web browser, curl, Postman, or any HTTP client.

Cross-origin resource sharing (CORS) : To define how different origins can access your function URL, use cross-origin resource sharing (CORS). We recommend configuring CORS if you intend to call your function URL from a different domain. Lambda supports the following CORS headers for function URLs.

After creating the function URL you can invoke your function using a web browser, curl, Postman, or any HTTP client.
Press enter or click to view image in full size


<img width="1426" height="467" alt="image" src="https://github.com/user-attachments/assets/89b84639-49f9-44ff-9b71-9caa4a23250c" />


Take the URL of the Lambda function and hit it in the browser and you will get the same JSON response like which have get it from API Gateway.
Press enter or click to view image in full size


<img width="824" height="173" alt="image" src="https://github.com/user-attachments/assets/7525c3d0-279f-4813-ba0b-c5ff8a3e0617" />

Conclusion :
You’re now ready to start creating your own highly performant APIs for your projects. 








