## AWS Step Functions Cross-account execution example

This project contains two [AWS
SAM](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-getting-started.html)
applications used to demonstrate cross-account invocation of Step Functions workflow using AWS Step Functions and Amazon API Gateway.

- `sam-step-functions-account-a` - SAM Application which creates a Step Functions workflow in the source account
- `sam-step-functions-account-b` - SAM application which creates a Step Functions workflow and API Gateway API in destination account

In this example, the AWS Account IDs are setup as follows:

- `111111111111` - The account which will own the source Step Functions workflow
- `222222222222` - This account will create destination Step Functions workflow and an API Gateway API

## Getting started

**Note**, this assumes you have [installed and configured AWS
SAM](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-getting-started.html) in your environment.

### Deploy Stepfunction workflow in account - #111111111111

Deploy the SAM application in the `sam-step-functions-account-a` directory.

```bash
$ cd sam-step-functions-account-a
$ sam build
$ sam deploy --guided
```

Take note of the `StepFunctionAccountA` value from the SAM output.

```
---------------------------------------------------------------------------------------------------------------------------------------------------
Outputs                                                                                                                                           
---------------------------------------------------------------------------------------------------------------------------------------------------
Key                 StepFunctionAccountA                                                                                                          
Description         Step Function State machine ARN                                                                                               
Value               arn:aws:states:us-east-1:111111111111:stateMachine:<stateMachineARN>                                           
---------------------------------------------------------------------------------------------------------------------------------------------------
```

### Deploy Step Functions workflow in AccountB - #222222222222

In this step, we will deploy Step Functions workflow in AccountB - #222222222222. Go to the project directory and use the following commands.

```bash
$ cd sam-step-functions-account-b
$ sam build
$ sam deploy --guided
#use the ARN from the above Outputs here in the SourceStateMachineArn
```

After deployment has finished, you will see API Url and StateMachineArn in the Outputs

```text
---------------------------------------------------------------------------------------------------------------------------------------------------
Outputs                                                                                                                                           
---------------------------------------------------------------------------------------------------------------------------------------------------
Key                 ApiGatewayAPI                                                                                                                 
Description         API Url                                                                                                                        
Value               abcdefghi.execute-api.us-east-1.amazonaws.com                                                                                

Key                 StepFunctionAccountB                                                                                                          
Description         Step Function State machine ARN                                                                                               
Value               arn:aws:states:us-east-1:222222222222:stateMachine:StateMachineName                                      
---------------------------------------------------------------------------------------------------------------------------------------------------
```

## Executing the workflow

Use the two values from the above Outputs in the below JSON as input to the Step Functions workflow in account A.

```
{
   "ApiUrl": "<API Url>",
   "stateMachineArn": "<StepFunctionAccountB ARN>",
   "body": "{\"someKey\":\"someValue\"}"
}
```

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.
