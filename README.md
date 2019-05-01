# Developing a Chatbot in AWS
This is a demo Chatbot in AWS that can be used to quickly get a high level understanding of the Implementation aspects of it. The Chatbot is integrated with Slack Channel and AWS Connect. The End User can interact with the chatbot via Web (Slack Channel) or Voice Channel (via AWS Connect). The Chatbot can handle 2 intents currently - Promotions and Agent Handoff.  

# High level Services and Components
The demo includes code for below components -
1. Lex Chabot (It has 2 intents - Promotions and ConnectToAgent)
2. Fulfillment Lambda Functions for Responding to the User Intents and initiating the Outbound Voice call in case of Agent-handoff Intent.
3. AWS Connect Contact Flow JSON

# High Level Architecture

![High Level Architecture of Chatbot on AWS](https://github.com/dhawalkp/AWSChatbot/blob/master/Architecture_ChatBot_AWS.png)

# Installation Steps

1. Create a Lex Bot in AWS
    *   Login to AWS Console and Open Lex Service Console. 
    *   Import the Lex bot by selecting Actions -> Import and select    https://github.com/dhawalkp/AWSChatbot/blob/master/OctankSmartBot_10_3edfe505-38b5-465a-9d21-f0d909891fb8_Bot_LEX_V1.zip 
    *   Follow the steps at https://docs.aws.amazon.com/lex/latest/dg/slack-bot-association.html to Integrate your Bot with    Slack Channel
 2. Import the Fulfilment Lambda Function
    * Import the lambda - https://github.com/dhawalkp/AWSChatbot/blob/master/AdPromo-Handler-03c6ec4d-9c4a-4c1b-8d7e-671e92d96f00.zip from your AWS Lambda Console. 
    *   Change the Lambda Code as below based on your Source Phone Number. This number is the end-user's can be reached phone number. The Lambda fn will initiate an Outbound Voice Call to Sample Call Center Toll Free number created by AWS Connect Service. Change the "number" attribute below -
        Markup :  `code()`
                var requestForOutBoundCall = {
                    "name": "Octank Call Center Toll Free ",
                    "number": "+18888888888",
                    "day": "Friday"
                };
  3. Import the Lambda Function to make Outbound Voice Call for Agent Hand-off Intent
    * Import the lambda - https://github.com/dhawalkp/AWSChatbot/blob/master/AdPromo-Call-Outbound-2b947465-6dbe-479c-99ff-49dcc095a8bc.zip
    * Get the ARN of this lambda 
  4. Configure the Fulfillment Lambda to call Outbound Voice call Lambda
    * Change the FunctionName in the below code of Fulfillment Lambda with the ARN of Outbound Voice Call Lambda that you imoported in the previous step
        Markup : `Code()`
            var responseForOutBoundCall = await lambda.invoke({
                    FunctionName: 'arn:aws:lambda:us-east-1:414210492846:function:AdPromo-Call-Outbound',
                    Payload: JSON.stringify(requestForOutBoundCall), // pass params,
                    InvocationType: 'Event'
            }
            

    Build and Deploy the Lex Bot and test your Bot using Slack Channel and see if its responding as below screenshot
    ![Slack Responses](https://github.com/dhawalkp/AWSChatbot/blob/master/Slack_Responses.png)
    
2. 
