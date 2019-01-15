# AWS Active Directory Assignment

[TOC]

## Intro

- We want to extend our on-premises Active Directory DomainControler to the AWS Cloud, meaning our AD Master is on-premise. The idea is to use the power of AWS and AWS Cloudformation to create templates that can be deployed at different environments.

- We manage all AD DS tasks ourselves in AWS the same as in on premises

## 1. Launch the  Stack

```shell
$ aws cloudformation create-stack \
--stack-name active-directory-stack \
--template-body file://ad_master.yaml \
--capabilities CAPABILITY_AUTO_EXPAND CAPABILITY_NAMED_IAM CAPABILITY_IAM
```

- to update the stack

```shell
$ aws cloudformation update-stack \
--stack-name active-directory-stack \
--template-body file://ad_master.yaml \
--capabilities CAPABILITY_AUTO_EXPAND CAPABILITY_NAMED_IAM CAPABILITY_IAM
```

## 4. Parameter

- Every template should have at least the following basic parameters

```yaml
Parameters:
  Owner:
    Description: Enter Team or Individual Name Responsible for the Stack.
    Type: String
    Default: Andreas Rose

  Project:
    Description: Enter Project Name.
    Type: String
    Default: invoicegenerator

  Subproject:
    Description: Enter Project Name.
    Type: String
    Default: active_directory

  EnvironmentName:
    Description: An environment name that will be prefixed to resource names
    Type: String
    Default: Dev
    AllowedValues:
      - Dev
      - Prod
    ConstraintDescription: Specify either Dev or Prod
```



## 5. Resources


### 5.1 Active Directory Resources

*Take the following as example:*

- at first we need an api gateway ressource
- it pulls its ressources (methods etc) from a swagger file sitting in an s3 bucket, which we uploaded before

```yaml
  MyApiGateway:
    Type: AWS::ApiGateway::RestApi
    Properties:
      Name: !Sub "${AWS::StackName}-MyApiGateway"
      Description: A description
      FailOnWarnings: true
      Body:
        Fn::Transform:
          Name: AWS::Include
          Parameters:
            Location: !Sub "s3://${AWS::AccountId}-${Project}-swaggerapis/lambda1_swagger.yaml"
```

- now we need a deployment for the api gateway

```yaml
  MyApiGatewayDeployment:
    Type: AWS::ApiGateway::Deployment
    Properties:
      RestApiId: !Ref MyApiGateway
      StageName: prod
```

- and finally the appropriate IAM Service Role for the API Gateway resource
- it allows the apigateway resource to invoke Lambda functions

```yaml
  MyApiGatewayRole:
    Type: AWS::IAM::Role
    Properties:
      AssumeRolePolicyDocument:
        Version: '2012-10-17'
        Statement:
          - Effect: Allow
            Principal:
              Service: apigateway.amazonaws.com
            Action: sts:AssumeRole
      Policies:
        - PolicyName: InvokeLambda
          PolicyDocument:
            Version: '2012-10-17'
            Statement:
              - Effect: Allow
                Action:
                  - lambda:InvokeFunction
                Resource:
                  - !GetAtt HelloLambda.Arn
```


### 5.2 Parameter Resources

TO BE ADDED

## 6. Test the Active Directory

### 6.1 check the access

*Take the following as example:*

- go to the Console - API Gateways
- check if the API Gateway is hooked up to Lambda by selecting the **POST** option under **/users** and then clicking **TEST**
- now we enter some testdata
- on the Test page set
  - **userId** = 123,
  - **Request Body**

```json
{
    "email": "myfancy@gmail.com"
}
```

- now click **Test**
- If everything worked, the **Status** should be **200** with no data

### 6.2 Test ABC

TO BE ADDED