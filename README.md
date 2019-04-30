# Developing a Chatbot in AWS
This is a demo Chatbot in AWS that can be used to quickly get a high level understanding of the Implementation aspects of it. The Chatbot is integrated with Slack Channel and AWS Connect. The End User can interact with the chatbot via Web (Slack Channel) or Voice Channel (via AWS Connect). The Chatbot can handle 2 intents currently - Promotions and Agent Handoff.  

# High level Services and Components
The demo includes code for below components -
1. Lex Chabot
2. Fulfillment Lambda Functions for Responding to the User Intents and initiating the Outbound Voice call in case of Agent-handoff Intent.
3. AWS Connect Contact Flow JSON

# High Level Architecture

![High Level Architecture of Chatbot on AWS](https://github.com/dhawalkp/AWSChatbot/Architecture_ChatBot_AWS.png)

