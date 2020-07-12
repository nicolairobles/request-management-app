# Request Management App
Open-source request management app that integrates with Google Sheets and Forms to handle administrative tasks like folder creation, brief creation, automated emails, trello card creation, & syncs updates from Trello. The live dashboard UI also provides a user-friendly way to manage requests and can Close or Archive requests on the spreadsheet and Trello.

The front-end (client side) uses VueJS and Bootstrap. The back-end uses ES6 Javascript that gets compiled and bundled to Apps Script compatible code using webpack.

# Live Demo
- [Live Dashboard](https://script.google.com/macros/s/AKfycbzgPXDj3PM1cKVB_Wf-AIu9MJL7uu2TwoGkL90KpfSp/exec)
  - A pop-up will say "This app isn't verified". Click 'Advanced' to continue.
- [Trello Board](https://trello.com/b/2nYmBREX/request-management-app-demo)
- [Form to Submit Requests](https://docs.google.com/forms/d/e/1FAIpQLSfi01auBACD93dRZmzlDoquNm0xArFcQ0fKDEwW3c9qiz9Yzw/viewform)


## Dashboard
![Dashboard](http://67.205.135.229/wp-content/uploads/2020/07/Screen-Shot-2020-07-12-at-6.43.53-PM-1024x478.png "Dashboard")

![Request Details](http://67.205.135.229/wp-content/uploads/2020/07/Screen-Shot-2020-07-12-at-6.42.19-PM-1024x471.png "Request Details")

![Trello Board](http://67.205.135.229/wp-content/uploads/2020/07/Screen-Shot-2020-07-12-at-6.37.07-PM-1024x474.png "Trello Board")

![Confirmation Email](http://67.205.135.229/wp-content/uploads/2020/07/Screen-Shot-2020-07-12-at-6.45.49-PM-1024x723.png "Confirmation Email")

![Completion Email](http://67.205.135.229/wp-content/uploads/2020/07/Screen-Shot-2020-07-12-at-6.46.03-PM-1024x696.png "Completion Email")

## Set Up

Install clasp command line tool (if you don't have it already)

```bash
npm install @google/clasp -g
```

Then login to your Google account:

```bash
clasp login
```
Open the terminal and clone this project.

```bash
npm install
```


## Other Set Up

1. Create a Google Form and corresponding Google Sheet to capture the responses
1.a. Add the following columns to the spreadsheet
```
Job Number (should be first column)
Shared Folder	Status	
Trello Card Id	
Trello Card Url	
Trello Card Data	
Brief Url	
Timestamp	(default)
Email Address	
Summary	
Project Name	
Due Date
```
1. b. Add a sheet called Brief to your Google Spreadsheet
2. Create a trello board and get a Trello Key and Token (https://trello.com/app-key)
3. Get the list id of where you want cards to arrive on your trello board
4. Create a Google Drive Folder where you want dedicated request folders to be stored and get its id
5. In your Google Sheet, go to Tools > Script Editor
5. a. Give your script a name
5. b. Go to File > Project Properties > Script properties and place the following key value pairs:
```
TRELLO_KEY
TRELLO_TOKEN
idList
mainDriveFolder
```
5. c. Also get your Script ID in File > Project Properties > Info and place the ID in .clasp.json
6. Adjust appscript.json to customize who you want accessing the app. Default is ANYONE signed in (https://developers.google.com/apps-script/manifest#webapp).
6. a. Make sure your Google Apps Script API is turned on: https://script.google.com/home/usersettings
7. Run
```
npm run deploy
```
8. You can customize the data that is pulled in to the requests views by editing getNewRequests() in server/src/classes/RequestApp.js and server/src/classes/QueryBuilder.js
9. Set up triggers for 
- onFormSubmit - onFormSubmit
- getInProgressRequestsAndUpdate - every minute
10. When accessing the app, you will be asked to authorize. If publicly available, Google will show a notice of the app being unverified. Click on Advanced and then the link to your app.
11. Ensure you share the script with anyone with the link. In File > Share.

## Project Structure

The client/src/pages contains the partial pages. See src/pages/routes.js where the routes are configured.
You can customize the global styles in src/pages/scss. This project uses bootstrap as CSS framework.

The server/src folder contains the server side (Apps Script) code. You can use ES6 or npm modules in lib.js and any classes/files included in lib.js. 
api.js exposes the Apps Script API that the client side calls (see client/src/services/GASBackEnd.js)


## How does it work?
In the front end, it uses webpack and babel to cross compile and generate the bundle.
Then it uses html-webpack-inline-source-plugin to inline the whole Javascript and CSS in to the generated index.html file.
You can see the client side setup in the client/webpack.prod.js 
When you close or archive a request in the Dashboard, it takes within one minute for the Trello Card to sync with the spreadsheet. This is why the request will say: "Pending Review"

## Future Enhancements
- There is a bug when a data column in Google Sheets is not plain text. For example, dates. I've added an interim solution that converts all those dates to plain text.

## Reference
Spreadsheet
![Spreadsheet](http://67.205.135.229/wp-content/uploads/2020/07/Screen-Shot-2020-07-12-at-6.24.48-PM-1024x455.png "Spreadsheet")

