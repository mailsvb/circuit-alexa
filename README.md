# circuit-alexa
Alexa skill for Circuit integration
## Setup skill

1. Get a valid Cookie via Chrome and "EditThisCookie" extension
2. Download skill.zip and edit index.js (Circuit server address and cookie information)
3. Deploy skill.zip to AWS Lambda
4. Configure Alexa skill

### Step 1: Get a valid cookie
1. Open Chrome and install "EditThisCookie" via the Chrome web store: https://chrome.google.com/webstore/detail/editthiscookie/fngmhnnpilhplaeedifhccceomclgfbg
2. Do a fresh login into your Circuit Account (Tick: "This is a private Computer")
3. Open "EditThisCookie" and copy the cookie information ![Get your Cookie](/pics/cookie.jpg?raw=true "Get your Cookie")

### Step 2: Edit zip file
1. After downloading skill.zip from this repository, extract the content of it
2. Edit index.js with a simple text editor. A line that starts with **let con = new Circuit** needst to be edited. The line can be found within the first few lines of the file.
3. name the server address (e.g. eu.yourcircuit.com) and add your session cookie information to "connect.sess="

### Step 3: AWS Lambda
1. Go to http://aws.amazon.com/lambda/ . You will need to set-up an AWS account (the basic one will do fine) if you don't have one already ** Make sure you use the same Amazon account that your Echo device is registered to** Note - you will need a credit or debit card to set up an AWS account - there is no way around this. If you are just using this skill then you are highly unlikely to be charged unless you are making at least a million requests a month!
2. Go to the AWS Console and click on the Lambda link. Go to the drop down "Location" menu and ensure you select US-East(N. Virginia) if you are based in the US or EU(Ireland) if you are based in the UK or Germany. This is important as only these two regions support Alexa. NOTE: the choice of either US or EU is important as it will affect the results that you get. The EU node will provide answers in metric and will be much more UK focused, whilst the US node will be imperial and more US focused.
3. Click on the Create a Lambda Function or Get Started Now button.
4. Skip the Select Blueprint Tab and just click on the "Configure Triggers" Option on the left hand side
5. On the Configure Triggers tab Click the dotted box and select "Alexa Skills Kit". Click Next  
6. Name the Lambda Function "Circuit".
7. Select the default runtime node.js 6.10
9. Select Code entry type as "Upload a .ZIP file" and upload your modified skill.zip file
10. Keep the Handler as index.handler (this refers to the main js file in the zip).
11. Create a basic execution role by slecting "Create new role from template(s)" in the Role box, then name the role "lambda_basic_execution” in the role name box and click create. (or Choose use an existing role if you have deployed skills previously and then select "lambda_basic_executuion" from the existing role dropdown ).
12. Under Advanced settings change the Timeout to 10 seconds
13. Click "Next" and review the settings then click "Create Function". This will upload the skill.zip file to Lambda.
14. Copy the ARN from the top right to be used later in the Alexa Skill Setup (it's the text after ARN - it won't be in bold and will look a bit like this arn:aws:lambda:eu-west-1:XXXXXXX:function:circuit).

### Step 4: Alexa skill
1. Go to the Alexa Console (https://developer.amazon.com/edw/home.html and select Alexa on the top menu)
2. Click "Get Started" under Alexa Skills Kit
3. Click the "Add a New Skill" yellow box.
4. You will now be on the "Skill Information" page. 
5. Set "Custom Interaction Model" as the Skill type
6. Select the language as English (US), English (UK), or German depending on your location
7. Set "Circuit" as the skill name and "circuit" as the invocation name, this is what is used to activate your skill. For example you would say: "Alexa, start circuit and read my last messages."
8. Leave the "Audio Player" setting to "No"
9. Click Next.
10. You will now be on the "Interaction Model" page. 
11. Copy the text below into the "Intent Schema" box.
```
{
  "intents": [
    {
      "intent": "GetLatestMessages"
    },
    {
      "intent": "GetUnread"
    },
    {
      "intent": "GetMentions"
    },
    {
      "intent": "AMAZON.HelpIntent"
    },
    {
      "intent": "AMAZON.StopIntent"
    },
    {
      "intent": "AMAZON.CancelIntent"
    }
  ]
}
```
12. In the Sample Uterances box, add a few utterances you would use to initiate a feature. One of "GetLatestMessages", "GetUnread" or "GetMentions". Below are a few examples for German langauge
```
GetLatestMessages Lies mir meine letzten Nachrichten vor
GetLatestMessages meine letzten Nachrichten
GetLatestMessages Was sind meine letzten Nachrichten
GetUnread Habe ich ungelesene Nachrichten
GetUnread ungelesene Nachrichten
GetUnread Gibt es neue Nachrichten
GetUnread neue Nachrichten
GetUnread Wie viele ungelesene Nachrichten habe ich
GetMentions Habe ich ungelesene Erwähnungen
GetMentions ungelesene Erwähnungen
GetMentions Gibt es neue Erwähnungen
GetMentions neue Erwähnungen
GetMentions Wie viele ungelesene Erwähnungen habe ich
AMAZON.HelpIntent Was kann ich tun
AMAZON.HelpIntent Was kannst Du tun
```
13. Click Next.
14. You will now be on the "Configuration" page.
15. Select "AWS Lambda ARN (Amazon Resource Name)" for the skill Endpoint Type.
16. Then pick the most appropriate geographical region (either US or EU as appropriate) and paste the ARN you copied in step 13 from the AWS Lambda setup. 
17. Select no for Account Linking, special permissions are not required
18. Click Next.
19. You can test the skill by typing a query into the Service Simulator field or on your actual Alexa device. There is no need to go anyfurther through the process i.e. submitting for certification.
