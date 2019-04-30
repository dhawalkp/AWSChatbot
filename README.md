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

1. Create a Lex Bot in AWS - 
    a. Login to AWS Console and Open Lex Service Console. 
    b. Import the Lex bot by selecting Actions -> Import and select  https://github.com/dhawalkp/AWSChatbot/blob/master/OctankSmartBot_10_3edfe505-38b5-465a-9d21-f0d909891fb8_Bot_LEX_V1.zip 
    c. Follow the steps at https://docs.aws.amazon.com/lex/latest/dg/slack-bot-association.html to Integrate your Bot with    Slack Channel
    d. Build and Deploy the Lex Bot and test your Bot using Slack Channel and see if its responding as below screenshot
    ![Slack Responses](https://github.com/dhawalkp/AWSChatbot/blob/master/Chatbot_Slack_responses.png)
    
