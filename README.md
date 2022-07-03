In this system you can save the form data to Google Sheets by Pure Coding

How to create an HTML form that stores the submitted form data in Google Sheets using plain 'ol JavaScript (ES6), Google Apps Script, Fetch and FormData.
1. Create a new Google Sheet
First, go to Google Sheets and Start a new spreadsheet with the Blank template.
Rename it Email Subscribers. Or whatever, it doesn't matter.
Put the following headers into the first row:
A	B	C	...
1	timestamp	email		
To learn how to add additional input fields, checkout section 7 below.

2. Create a Google Apps Script
Click on Tools > Script Editor… which should open a new tab.
Rename it Submit Form to Google Sheets. Make sure to wait for it to actually save and update the title before editing the script.
Now, delete the function myFunction() {} block within the Code.gs tab.
Paste the following script in it's place and File > Save:
