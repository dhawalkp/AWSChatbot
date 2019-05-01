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
    *    Import the lambda - https://github.com/dhawalkp/AWSChatbot/blob/master/AdPromo-Handler-03c6ec4d-9c4a-4c1b-8d7e-671e92d96f00.zip from your AWS Lambda Console. 
    *   Change the Fullfilment Lambda Code as below based on your Source Phone Number. This number is the end-user's can be reached phone number. The Lambda fn will initiate an Outbound Voice Call to Sample Call Center Toll Free number created by AWS Connect Service. Change the "number" attribute below -
        `code()`
            
                var requestForOutBoundCall = {
                    "name": "Octank Call Center Toll Free ",
                    "number": "+18888888888",
                    "day": "Friday"
                };
  3. Import the Lambda Function to make Outbound Voice Call for Agent Hand-off Intent
     *    Import the lambda - https://github.com/dhawalkp/AWSChatbot/blob/master/AdPromo-Call-Outbound-2b947465-6dbe-479c-99ff-49dcc095a8bc.zip
     *    Get the ARN of this lambda 
    
  4. Configure the Fulfillment Lambda to call Outbound Voice call Lambda
      *    Change the FunctionName in the below code of Fulfillment Lambda with the ARN of Outbound Voice Call Lambda that you imoported in the previous step
        `code()`
         
            var responseForOutBoundCall = await lambda.invoke({
                    FunctionName: 'arn:aws:lambda:us-east-1:414210492846:function:AdPromo-Call-Outbound',
                    Payload: JSON.stringify(requestForOutBoundCall), // pass params,
                    InvocationType: 'Event'
            }
  6.  Go to "Connect" Service from your AWS Console and perform below steps
      *     Create call center instance
      *     Enable Inbound and Outbound calls
      *     Go to Integration section and add Lex Bot that you created in previous steps.
      *     In Integration section,  add the Fulfillment Lambda function that you created in previous steps.
      *     Login to Amazon Connect - Call Center Administrator console
      *     Go to Routing-> Manage Phone Numbers and Claim the Number for serving as an Inbound Call Center number for your end users
      *     Go to Routing -> Contact Flows and Import the https://github.com/dhawalkp/AWSChatbot/blob/master/ML%20Builders%20Day%20-%20Connect%20Demo JSON to import the Contact flow 
      *     Open the contact flow named "ML Builders Day - Connect Demo" and click on "Customer Input" Dialog box and make sure the lex bot name is configured correctly.
      *     Set the valid Phone number in "Transfer to Phone Number" dialog which will serve as the Agent Phone number which will be triggered when "ConnectToAgent" intent is triggered by the lex bot integrated with the Contact flow.
      
  5.  Change the Outbound Voice call  Lambda Code to reflect correct InstanceID of your call Center instance in your Amazon Connect service. The Contact Flow ID can be retrieved by clicking "Show additional Flow Information" in your contact flow designer UI. The Source Phone Number is your Call Center Phone Number that you claimed in the Connect.
        `code()`   
        
               let params = {
                  "InstanceId" : '298c8642-3d37-489f-bf85-cfb2c354c4b8',
                  "ContactFlowId" : 'fae8fab1-014e-44cf-927c-a2d55ae22b22',
                  "SourcePhoneNumber" : '+18888888888',
                  "DestinationPhoneNumber" : customerPhoneNumber,
                  "Attributes" : {
                        'name' : customerName,
                        'dayOfWeek' : dayOfWeek
                  }
        
               }
6.    Set the IAM roles of your "Outbound Voice call" Lambda function to make sure they have Policy to call Connect via adding policy - arn:aws:iam::aws:policy/AmazonConnectFullAccess 
7.    Set the IAM role of your fullfillment Lambda function to be able to call "Outbound Voice call" lambda by making sure the IAM policy for that role has the access to call other lambda functions.
8.    Build and Deploy the Lex Bot and test your Bot using Slack Channel and see if its responding as below screenshot
    ![Slack Responses](https://github.com/dhawalkp/AWSChatbot/blob/master/Slack_Responses.png)
    
