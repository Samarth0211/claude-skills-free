---
name: lambda-function
description: Create AWS Lambda function with handler
category: cloud
tags: ["cloud", "aws", "lambda"]
difficulty: intermediate
version: 1.0.0
author: Claude Skills Hub
---

# Lambda Function

> Create AWS Lambda function with handler

You are an AWS developer. The user wants to create an AWS Lambda function with a handler that responds to events and can be deployed and tested.

## What to check first
- Run `aws --version` to confirm AWS CLI is installed and configured
- Verify IAM permissions include `lambda:CreateFunction`, `lambda:UpdateFunction`, and `iam:PassRole`
- Check that you have an execution role ARN ready or create one with `aws iam create-role --role-name lambda-execution-role --assume-role-policy-document file://trust-policy.json`

## Steps
1. Create a `trust-policy.json` file that allows Lambda to assume the role: include `"Service": "lambda.amazonaws.com"` in the principal
2. Create the execution role with appropriate permissions (attach `AWSLambdaBasicExecutionRole` for CloudWatch logs)
3. Write your handler function in a file like `index.js` or `lambda_function.py` with the correct function signature for your runtime
4. Create a `deployment.zip` file containing only your handler code: `zip deployment.zip index.js` (do not include node_modules or venv)
5. Use `aws lambda create-function` with `--handler`, `--role`, `--runtime`, and `--zip-file` parameters
6. Test the function locally with `aws lambda invoke --function-name <name> --payload '{}' response.json`
7. View logs in CloudWatch with `aws logs tail /aws/lambda/<function-name> --follow`
8. Update the function code with `aws lambda update-function-code --function-name <name> --zip-file fileb://deployment.zip` after changes

## Code
```javascript
// index.js - Lambda handler for Node.js 18.x
exports.handler = async (event, context) => {
  console.log('Event received:', JSON.stringify(event, null, 2));
  
  try {
    // Extract data from event
    const body = event.body ? JSON.parse(event.body) : event;
    const name = body.name || 'World';
    
    // Process the request
    const message = `Hello, ${name}!`;
    
    // Return response in API Gateway format
    return {
      statusCode: 200,
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        message: message,
        timestamp: new Date().toISOString(),
        requestId: context.requestId
      })
    };
  } catch (error) {
    console.error('Error processing request:', error);
    return {
      statusCode: 500,
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        error: 'Internal server error',
        message: error.message
      })
    };
  }
};

// AWS CLI deployment script (deploy.sh)
#!/bin/bash
```

*Note: this example was truncated in the source. See [the GitHub repo](https://github.com/Samarth0211/claude-skills-hub) for the latest full version.*

## Common Pitfalls

- Treating this skill as a one-shot solution — most workflows need iteration and verification
- Skipping the verification steps — you don't know it worked until you measure
- Applying this skill without understanding the underlying problem — read the related docs first


## When NOT to Use This Skill

- When a simpler manual approach would take less than 10 minutes
- On critical production systems without testing in staging first
- When you don't have permission or authorization to make these changes


## How to Verify It Worked

- Run the verification steps documented above
- Compare the output against your expected baseline
- Check logs for any warnings or errors — silent failures are the worst kind


## Production Considerations

- Test in staging before deploying to production
- Have a rollback plan — every change should be reversible
- Monitor the affected systems for at least 24 hours after the change



---
*From [CLSkills.in](https://clskills.in/browse) — 2,300+ free Claude Code skills*

