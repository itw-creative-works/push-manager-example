# Push Manager Example

## Get Started
### 1. Clone
Clone this repository to your computer via GitHub desktop or command line
```shell
# Clone this repository
git clone https://github.com/itw-creative-works/push-manager-example
# Go into the repository
cd push-manager-example
```

### 2. Create Firebase project
Create a Firebase project in your firebase console [https://console.firebase.google.com](https://console.firebase.google.com). For example, we'll call our project `push-manager-example`.
You may also have to manually create a Firestore database by going to 'Database' on the left panel and then clicking 'Create database', and then selecting 'Test mode'.

### 3. Initialize Firebase
Initialize Firebase
```shell
cd push-manager-example
init firebase
```

### 4. Select Firebase services
Select Firestore, Functions, Hosting, then press `enter`.
```shell
◯ Database: Deploy Firebase Realtime Database Rules
◉ Firestore: Deploy rules and create indexes for Firestore
◉ Functions: Configure and deploy Cloud Functions
◉ Hosting: Configure and deploy Firebase Hosting sites
◯ Storage: Deploy Cloud Storage security rules
```

### 5. Choose your Firebase project
Select our project titled `push-manager-example`.
```shell
? Select a default Firebase project for this directory:
  awesome-project-1 (Awesome Project 1)
❯ push-manager-example (push-manager-example)
  awesome-project-2 (Awesome Project 2)
```

### 6. Continue setting up your Firebase Project
* What file should be used for Firestore Rules? > ``
* File firestore.rules already exists. Do you want to overwrite it with the Firestore Rules from the Firebase Console? > `y`
* What file should be used for Firestore indexes? > ``
* What language would you like to use to write Cloud Functions? > `JavaScript`
* Do you want to use ESLint to catch probable bugs and enforce style? > `n`
* File functions/package.json already exists. Overwrite? > `n`
* File functions/index.js already exists. Overwrite? > `n`
* Do you want to install dependencies with npm now? > `y`
* What do you want to use as your public directory? > `.`
* Configure as a single-page app (rewrite all urls to /index.html)? > `n`
* File ./404.html already exists. Overwrite? > `n`
* File ./index.html already exists. Overwrite? > `n`


### 7. Install functions dependencies
Navigate to the functions folder and install npm modules.
```shell
cd functions
npm install
```

### 8. Deploy the project
Navigate to the root project directory and deploy the project
```shell
cd ..
firebase deploy
```
After it has finished deploying, navigate to [https://push-manager-example.web.app/](https://push-manager-example.web.app/) (replace `push-manager-example` with your project ID).

### 9. Subscribe and Send Notifications
Click 'Subscribe' to add your current browser's token to your Firestore. You should see a popup that requests permission to receive notifications, so click 'Yes', or 'Enable'.

Then, click 'Send Notification' to send a test push notification. There are a couple of things to keep in mind here.
* Most browsers will not display a push notification if the website is on an active tab. To combat this, there is a **3 second delay** implemented in the client side code so you should switch tabs or minimize the browser after you click the button.
* If you are running this project locally, it will probably not send the notification due to a lack of feature implementation on Firebase. So you should run `firebase deploy` over `firebase serve`.


## Further learning
### Firestore Document Structure for Tokens
This is the required format for the subscription document in Firestore. Here, the `token` is stored so Firebase knows *where* to send the notifications.
```js
// notifications/subscriptions/all/{subscriptionId}
{
  token: 'tokenId',
}
```

### Firestore Document Structure for Notifications
This is the required format for the notification document in Firestore. Here, the notification `payload` is stored so Firebase knows *what* to send in the notifications.
```js
// notifications/processing/all/{notificationId}
{
  payload: {
    icon: 'https://upload.wikimedia.org/wikipedia/commons/5/53/Google_%22G%22_Logo.svg', // Link to notification icon
    click_action: 'https://google.com', // URL click action
    title: 'Hello world!',
    body: 'This is my first push notification using Push Manager!',
  }  
}
```

The Firestore documents for both the *tokens* and the *notifications* can have other data, but both must *at least* have the above data for push-manager to work properly.
