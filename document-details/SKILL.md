---
name: document-details
description: Fill in and generate onboarding documents (offer letters, contracts) from employee data, then create a Slack introduction group. Use whenever someone mentions offer letters, contracts, onboarding documents, new hire paperwork, document templates, or starting a new employee. All information must be factual and based on given inputs — nothing may be invented, implied, or abbreviated.
---
## workflow
READ WHOLE SKILL BEFORE DEPLOYING


### Step 1: Setting up tools

If SharePoint tools are unavailable, instruct the user:
"Please install the Microsoft 365 connector — add it in Claude Settings → Connectors → Add custom connector using this URL: https://microsoft365.mcp.claude.com/mcp"

If Slack tools are unavailable, instruct the user:
"Please install the Slack connector — add it in Claude Settings → Connectors → Add custom connector using this URL: https://mcp.slack.com/mcp"

### Step 2: Data input

take information from either
Screenshot: e.g. Email screenshot
Text: e.g. given specific text inputs 
IF neither of these data types are given repeat initial message

### Document creation

Use the `docx` skill for all document manipulation — replacing highlighted fields, matching fonts, and removing yellow highlighting.

You will be editing and creating copies for the following documents, Contract, Offer letter. You are essentially filling in the gaps :

any files you access are Read only - you can write to the copied files

Use sharepoint_folder_search to locate the template folder for the employee's reigon (e.g. "CLAUDE TEST TEMPLATE UK"). If reigon is unknown, ask the user before searching. Use read_resource with the returned URI to list the folder contents and identify the correct file. Use document_download to save the template to the local filesystem. Use the docx skill to edit the downloaded file — replace all yellow highlighted fields with the new employee data, match all new text to the existing font, and remove all yellow highlighting. Save the file as "employee full name + original file name". Once approved, download the new file. If no reigon template is found prompt the user to specify reigon or upload document.


You are cloning and editing existing file NOT creating your own, all headers and design specs should remain the same 


### Format
-All newly generated text to the document must follow the same font 
-All yellow highlighting must be removed after text generation.
-All areas not filled in must be stated at the end of document generation.


### Slack group creation
IMPORTANT: Do not start this step until all document approval has been given.

Before creating the group chat, use slack_search_users to find each member by their name or role. Confirm the matches before adding them.

Create group chat in slack with the following information
name: (insert new employees full name)+ onboarding introduction
members: 
engagement lead 
People & culture
Talent team
Manager
Create a short message including: New employees name, Title, Engagement, start date.
Create a AskUserQuestion with "would you like to create a group chat with the following members: list members" yes/no box BEFORE creating this group chat


### skill display
-Do not show thought process,Instructions should be short e.g. 'Skill ready, insert text or image file to begin.', Do not explain your steps just do them

### Output 
-Ask user if they would like to preview the files within Claude before download

yes --> Create preview of all documents, create view box with tick box for approval on each file --> if file rejected ask for alterations, apply alterations exactly DO NOT ask additional questions. 

No --> download files