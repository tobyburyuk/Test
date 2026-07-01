---
name: document-details
description: Downloading file templates utilising sharepoint and chrome extension. Filling in and generate onboarding documents (offer letters, contracts) from employee data, then create a Slack introduction group. Final step is to schedule reminders for probation meetings. Use whenever someone mentions offer letters, contracts, onboarding documents, new hire paperwork, document templates, or starting a new employee. All information must be factual and based on given inputs — nothing may be invented, implied, or abbreviated.
---
## workflow
READ WHOLE SKILL BEFORE DEPLOYING

### skill display
THE FOLLOWING SECTION APPLIES TO THE ENTIRE SKILL, NOT JUST THE DOCUMENT CREATION PORTION
-Do not show thought process,Instructions should be short e.g. 'Skill ready, insert text or image file to begin.', Do not explain your steps just do them
-Do not narrate actions. Do not announce what you are about to do. Execute silently and only speak when you need input from the user or have a final result to present.

### Step 1: Setting up tools

If SharePoint tools are unavailable, instruct the user:
"Please install the Microsoft 365 connector — add it in Claude Settings → Connectors → Add custom connector using this URL: https://microsoft365.mcp.claude.com/mcp"

If Slack tools are unavailable, instruct the user:
"Please install the Slack connector — add it in Claude Settings → Connectors → Add custom connector using this URL: https://mcp.slack.com/mcp"

If Chrome browser tools are unavailable, instruct the user:
"Please install the Claude in Chrome extension — add it from the Chrome Web Store: https://chromewebstore.google.com/detail/claude/fcoeoabgfenejglbffodgkkbkcdhcgfn then restart Claude desktop to connect it."

### Step 2: Data input

take information from either
Screenshot: e.g. Email screenshot
Text: e.g. given specific text inputs 
IF neither of these data types are given repeat "Skill ready — please paste employee details or share a screenshot to begin."

### Document creation

Use the `docx` skill for all document manipulation — replacing highlighted fields, matching fonts, and removing yellow highlighting.

You will be editing and creating copies for the following documents, Contract, Offer letter. You are essentially filling in the gaps :

any files you access are Read only - you can write to the copied files

Use sharepoint_folder_search to locate the template folder for the employee's region (test/testing/test). If region is unknown, ask the user before searching. Use read_resource with the returned URI to list the folder contents and identify the correct file. Use read_resource with the file URI to fetch the document content, — replace all yellow highlighted fields with the new employee data, match all new text to the existing font, and remove all yellow highlighting. Save the file as "employee full name + original file name". Once approved, download the new file. If no region template is found prompt the user to specify region or upload document.

(read each step before executing, do not skip any steps)
Step 1 — Load Chrome tools
Load via ToolSearch: select:mcp__claude-in-chrome__tabs_context_mcp,mcp__claude-in-chrome__navigate,mcp__claude-in-chrome__computer,mcp__claude-in-chrome__find,mcp__claude-in-chrome__get_page_text

Step 2 — Navigate to the template folder
Use the SharePoint folder URI returned by sharepoint_folder_search in the document creation step. Extract the webUrl from that result and navigate to its parent folder in Chrome. If no URI was found, ask the user to manually upload the document to claude. 

Step 3 — Locate and download the file,
Read the folder contents using get_page_text to identify the correct template filename
Right-click the target file → Download, Wait 3 seconds for the download to complete

Step 4 - STOP — DO NOT PROCEED until folder access is confirmed. After download, immediately call mcp__cowork__request_cowork_directory for ~/Downloads. If this fails or is skipped, halt and tell the user before doing anything else.
If the template file cannot be accessed via the shell, stop immediately and tell the user. Never recreate a document from scratch as a workaround — all formatting and branding will be lost.

Step 5 — Save to a consistent output folder
Create the output folder if it doesn't exist: ~/Downloads/ClearRoute Onboarding/

— replace all yellow highlighted fields with the new employee data, match all new text to the existing font, and remove all yellow highlighting. Save the file as "employee full name + original file name". Once approved, download the new file. If no region template is found prompt the user to specify region or upload document.

You are cloning and editing existing file NOT creating your own, all headers and design specs should remain the same 

### Documents produced
->Offer letter
->Contract letter
->Probation completion letter

Make sure all 3 documents are produced and approved before continuing, unless user specifies otherwise.


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

### Slack Scheduling 
use slack_schedule_message to set up reminders for the manager to set up probation meetings at 2 different dates

1 - 5 weeks after start date, message: :alert: Reminder [insert employees name] is coming up for their mid-probation checkin, ensure this is scheduled by [insert date 6 weeks after start]
2 - 11 weeks after start date, message: :alert: Reminder [insert employees name] is coming up for their end-of-probation checkin, ensure this is scheduled by [insert date 12 weeks after start], Include [Probation letter] as an attachment.


### Output 
-Ask user if they would like to preview the files within Claude before download

yes --> Create preview of all documents, create view box with tick box for approval on each file --> if file rejected ask for alterations, apply alterations exactly DO NOT ask additional questions. 

No --> download files