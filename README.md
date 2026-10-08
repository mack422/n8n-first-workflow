# n8n First Workflow: Account Health Check

My first n8n automation. A form takes five numbers for a customer account and returns a Green, Yellow or Red health rating with the reasons and a recommended action.

I built it to learn how n8n moves data between nodes. The idea comes from the daily account monitoring I do on my own book of business. See my [AI account health agent](https://github.com/mack422/ai-account-health-agent) for the larger version of the same idea.

## What it does

The form asks for the account name, CSAT, NPS, days to renewal and days since the last contact. A scoring step adds up risk points for each weak signal and gives extra weight to a renewal inside 60 days. The result is a status, the reasons behind it and one next action.

Example, a Red result:
Low CSAT, Low NPS, No contact in over 30 days, Renewal inside 60 days. Action: call the daily user lead today and update the sponsor.

Example, a Green result:
No flags. Action: keep the normal cadence and look for expansion.

## Nodes

1. **On form submission** (Form Trigger). Collects the five inputs.
2. **Code in JavaScript** (Code). Scores the account and writes the status, reasons and action.

## Scoring rules

These cutoffs are example values I chose for the demo. They are not rules from any employer. The code is easy to edit.

CSAT below 7 adds 2 points, and 7 or 8 adds 1.
NPS below 6 adds 2 points, and 6 or 7 adds 1.
More than 30 days since contact adds 2 points, and more than 21 adds 1.
A renewal inside 60 days adds 1 point when any other flag exists.
Four or more points is Red, two or three is Yellow, and fewer is Green.

## Run it

Import `workflow.json` into n8n (in the editor, paste the file's contents onto the canvas), then click Test workflow and submit the form. No outside accounts or credentials are needed.

## What I learned

Form field names have to match the names used in the code exactly. Data moves through n8n as items, and a small amount of JavaScript handles logic that would take many visual nodes. Testing a Red and a Green case caught label mistakes early.

## Next

Send the result to Slack, and read live data from a spreadsheet so the workflow scores a whole book of business.
