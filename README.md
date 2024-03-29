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



save the file 


var sheetName = 'Sheet1'
var scriptProp = PropertiesService.getScriptProperties()

function intialSetup () {
  var activeSpreadsheet = SpreadsheetApp.getActiveSpreadsheet()
  scriptProp.setProperty('key', activeSpreadsheet.getId())
}

function doPost (e) {
  var lock = LockService.getScriptLock()
  lock.tryLock(10000)

  try {
    var doc = SpreadsheetApp.openById(scriptProp.getProperty('key'))
    var sheet = doc.getSheetByName(sheetName)

    var headers = sheet.getRange(1, 1, 1, sheet.getLastColumn()).getValues()[0]
    var nextRow = sheet.getLastRow() + 1

    var newRow = headers.map(function(header) {
      return header === 'timestamp' ? new Date() : e.parameter[header]
    })

    sheet.getRange(nextRow, 1, 1, newRow.length).setValues([newRow])

    return ContentService
      .createTextOutput(JSON.stringify({ 'result': 'success', 'row': nextRow }))
      .setMimeType(ContentService.MimeType.JSON)
  }

  catch (e) {
    return ContentService
      .createTextOutput(JSON.stringify({ 'result': 'error', 'error': e }))
      .setMimeType(ContentService.MimeType.JSON)
  }

  finally {
    lock.releaseLock()
  }
}
