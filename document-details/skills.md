---
name: document-details
description: fill in multiple documents with given details (documents include- offer letters, contracts), look for 'offer letter', 'onboarding', 'contracts'. All information used must be factual and based of given inputs, no data can be made up, implied or abreviated.
---
## workflow
READ WHOLE SKILL BEFORE DEPLOYING
### Step 1: Data input

take information from either
Screenshot: e.g. Email screenshot
Text: e.g. given specific text inputs 
IF neither of these data types are given repeat inital message


### Step 2: Setting up tools

If SharePoint tools are unavailable, instruct the user:
"Please install the Microsoft 365 connector — add it in Claude Settings → Connectors → Add custom connector using this URL: https://microsoft365.mcp.claude.com/mcp"

If Slack tools are unavailable, instruct the user:
"Please install the Slack connector — add it in Claude Settings → Connectors → Add custom connector using this URL: https://mcp.slack.com/mcp"

### Document creation
You will be editing and creating copies for the following documents, Contract, Offer letter,... :

any files you acces are Read only - you can write to the copied files

Use sharepoint_folder_search with name "Offer Letters and Contracts" to locate the template folder. Use read_resource with the returned URI to list the folder contents and select the appropriate file. If no result, ask the user to upload the template file directly.

example: UK = CR UK Offer Letter Template.docx

example:

Create copy of each document: 
All Yellow highlighted fields must be replaced with the new employee data.
The new documents must be saved as "employee full name + original file name. 


### Format
-All newly generated text to the document must follow the same font 
-All yellow highlighting must be removed after text generation.
-All areas not filled in must be stated at the end of document generation.


### Slack group creation
IMPORTANT: Do not start this step untill all document approval has been given.

Create group chat in slack with the following information
name: (insert new employees full name)+ onboarding introduction
members: 
engagement lead 
People & culture
Talent team
Manager
Create a short message including: New employees name, Title, Engagement, start date.
Create a Veiw tab with "would you like to create a group chat with the following members: list members" yes/no box BEFORE creating this group chat


### skill display
-Do not show thought process
-Instructions should be short e.g. 'Skill ready, insert text or image file to begin.'
-Do not explain your steps just do them

### Output 
-Ask user if they would like to preview the files within Claude before download

yes --> Create preview of all documents, create veiw box with tick box for approval on each file --> if file rejected ask for alterations, apply alterations exactly DO NOT ask additional questions. 

No --> download files