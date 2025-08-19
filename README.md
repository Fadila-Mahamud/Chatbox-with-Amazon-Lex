# Chatbot-with-Amazon-Lex
Creating a chatbot to assist with order tracking
Read.Me
 
# 🤖 Serverless Chatbot with Amazon Lex V2 & Lambda
 
This project demonstrates how to build a **serverless AI-powered chatbot** using:

- **Amazon Lex V2** for ASR + NLU

- **AWS Lambda** for fulfillment logic

- **CloudWatch** for monitoring
 
The chatbot can answer **order tracking queries** and be extended for other customer support functions.
 
---
 
## 🚀 Features

- Natural language understanding with **Amazon Lex V2**

- Dynamic intent handling with **AWS Lambda**

- Slot filling for capturing `OrderID`

- Serverless deployment (pay only for what you use)

- Easily integratable with websites, apps, or messaging platforms
 
---
 
## 🏗️ Architecture

1. User enters query (e.g., *"Track my order 12345"*).

2. Lex V2 processes intent → passes slot data to Lambda.

3. Lambda runs fulfillment logic → returns a response.

4. Lex sends the response back to the user.
 
 
# 🤖 Serverless Chatbot with Amazon Lex V2 & Lambda
 
This project demonstrates how to build a **serverless AI-powered chatbot** using:

- **Amazon Lex V2** for ASR + NLU

- **AWS Lambda** for fulfillment logic

- **CloudWatch** for monitoring
 
The chatbot can answer **order tracking queries** and be extended for other customer support functions.
 
---
 
## 🚀 Features

- Natural language understanding with **Amazon Lex V2**

- Dynamic intent handling with **AWS Lambda**

- Slot filling for capturing `OrderID`

- Serverless deployment (pay only for what you use)

- Easily integratable with websites, apps, or messaging platforms
 
---
 
## 🏗️ Architecture

1. User enters query (e.g., *"Track my order 12345"*).

2. Lex V2 processes intent → passes slot data to Lambda.

3. Lambda runs fulfillment logic → returns a response.

4. Lex sends the response back to the user.

---
 
## ⚙️ Setup Instructions
 
### 1. Create the Lex V2 Bot
- Go to AWS Lex Console → Create Bot.
- Select **Standard Bot Builder**.
- Name: `CustomerSupportBot`.
- Language: `English (US)`.

### 2. Add Intent
- Intent name: `CheckOrderStatus`
- Sample Utterances:
  - *Where is my order?*
  - *Track my order*
  - *What is the status of order {OrderID}*
- Slot: `OrderID` → Type `AMAZON.AlphaNumeric`
- Fulfillment: AWS Lambda

<img width="1366" height="716" alt="intent 1" src="https://github.com/user-attachments/assets/efb2f158-d84d-473d-8e04-58dab2de6c0b" />



### 3. Deploy Lambda
Upload the function in `lambda/lex_order_handler.py`.

<img width="1366" height="714" alt="lambda" src="https://github.com/user-attachments/assets/b227d17b-e9db-441a-b20d-c4eeab5ef7ad" />


 
### 4. Attach Lambda to Bot Alias
- Go to Lex → Bot Aliases → TestBotAlias → Locale → Lambda Code Hooks.
- Attach the Lambda.

<img width="825" height="436" alt="image" src="https://github.com/user-attachments/assets/bb4e9b3d-48b3-4130-b5bc-da4b10a77f70" />

 
### 5. Test in Lex Console
- Input: *"Track my order 12345"*
- Output: *"Order 12345 is currently being processed and will be delivered soon."*

<img width="832" height="434" alt="image" src="https://github.com/user-attachments/assets/47f229a7-4cd3-4508-ad76-b076a1c93a9b" />

 



---
 
## ⚙️ Setup Instructions
 
### 1. Create the Lex V2 Bot

- Go to AWS Lex Console → Create Bot.

- Select **Standard Bot Builder**.

- Name: `CustomerSupportBot`.

- Language: `English (US)`.
 
### 2. Add Intent

- Intent name: `CheckOrderStatus`

- Sample Utterances:

  - *Where is my order?*

  - *Track my order*

  - *What is the status of order {OrderID}*

- Slot: `OrderID` → Type `AMAZON.AlphaNumeric`

- Fulfillment: AWS Lambda
 
### 3. Deploy Lambda

Upload the function in `lambda/lex_order_handler.py`.
 
### 4. Attach Lambda to Bot Alias

- Go to Lex → Bot Aliases → TestBotAlias → Locale → Lambda Code Hooks.

- Attach the Lambda.
 
### 5. Test in Lex Console

- Input: *"Track my order 12345"*

- Output: *"Order 12345 is currently being processed and will be delivered soon."*
 
---
 
## 📝 Lambda Code
 
`lambda/lex_order_handler.py`

```python

import json
 
def lambda_handler(event, context):

    print("Incoming event:", json.dumps(event))
 
    intent = event['sessionState']['intent']['name']

    slots = event['sessionState']['intent']['slots']
 
    if intent == "CheckOrderStatus":

        order_id = None

        if slots and 'OrderID' in slots and slots['OrderID']:

            order_id = slots['OrderID']['value']['interpretedValue']

        if order_id:

            response_text = f"Order {order_id} is currently being processed and will be delivered soon."

        else:

            response_text = "I couldn't find an order ID. Can you provide it again?"

    else:

        response_text = "Sorry, I didn’t understand that request."
 
    return {

        "sessionState": {

            "dialogAction": {"type": "Close"},

            "intent": {"name": intent, "state": "Fulfilled"}

        },

        "messages": [

            {"contentType": "PlainText", "content": response_text}

        ]

    }

 
 
 
