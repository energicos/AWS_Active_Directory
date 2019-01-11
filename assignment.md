# Assignment

## A. Introduction

### 1. Objective

The goal is to setup an Active Directory for a Microsoft Server that will be deployed at AWS.
We want to connect AWS to an existing On Premise Microsoft Active Directory, and it's mandatory to avoid using the fully managed AD solution. Please use the bootstraped version of the AD and Powershell to install the features and roles.

### 2. Technologies to be used

- AWS Cloudformation

Cloudformation is a service of AWS that allows you to automate the infrastructure.
The deliveriables need to be written in Cloudformation yaml files.

### 3. Provided by us

- YAML templates containing some resources needed for the Active Directory
- Follow this guide from the AWS official documentation https://docs.aws.amazon.com/quickstart/latest/active-directory-ds/scenario-2.html
- [Here](https://github.com/aws-quickstart/quickstart-microsoft-activedirectory) you can find the assets related to the previous scenario, using cloudformation.

## B. Details of the assignment

### 1. Setup
You should create an AWS acccount on your onwn. There's a basic plan that will allow you to implement this assignment with no charges.
Nonetheless be careful, there are resources that are not covered by the basic plan.

### 2. Install the AWS CLI
You need to install the AWS CLI in your PC, so you can execute Cloudformation scripts.

### 3. Timeline
- Project Start: 14.01.2019
- the timeframe for the project is set to 15 days upon hiring the developer
- meaning the assignment must be ready ==29.01.2019==