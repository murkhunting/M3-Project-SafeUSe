<h1>SAFE-USE</h1> <img src="C:\Users\troy1\Desktop\Logo.png" alt="Logo" style="zoom:8%;" />
## Description

The mission of Safe Use is to provide education to prevent drugs abuse. As well as safety and a quick access to support for people having a challenging experience.

## User Flow

- **404:** As an anon/user I can see a 404 page if I try to reach a page that does not exist so that I know it's my fault

- **Homepage**: As a public user we can see the main with a brief explanation and the links to login and sign up to get started.

- **Login:** As an user I can login to start using the app. If i don't have an account yet i can click on the sign up button and get to the sign up page to create a new account.

- **Sign up:** As public user I can sign up filling in the form to start using the app. You get logged directly and you get redirected to the Homepage.

- **Homepage:** As a logged user now I can see a navigation bar and travel from the main to the other pages of the app: Profile, Learn, Experience and History 

- **Profile:** As a logged user I can see all my information and two buttons, one for editing the profile information, and another to logout.

  -  **Profile Edit:** As a logged user I can edit all the information on my profile and add new pathologies to prevent bad experiences.
  -  **Logout:** As a logged user I can click on the logout button in the profile page and logout from the platform.

- **Learn**: As a logged user I can see all the substances in the data base with a brief explanation about. By clicking on a substance you get redirected to the page of the selected substance.

  -  **Substance:** As a logged user I can see the complete information of the selected substance.

- **Experiences History:** As a logged user you can see a list of your experiences ordered by date of creation. If you click on one of them you get redirected to the page of the selected experience.

  -  **Experience Details:** in this page you can see all the details of the experience: date, duration, substances, mood, feeling, and all the notes that you have taken.

- **Experience Start:**  As a logged user you will start the process with 4 questions: "How are you feeling?", "Which mood describes you best right now?", "what is your intention?" and "have you eaten?". Then you can choose the substance you are going to take. And finally the app will recommend you a dose, will send you warnings and will inform you about the max dose you shold take, everything based on the answers that you introduced and your personal data. 

  When all this process is finished you will be able to click on the Start button that will redirect you to the tracking page.

  -  **Experience Track:** In this page you will have the warnings about your substance and the substances you should not mix. then you will be able to insert other substances to have the record




## Backlog 

- **Emergency call and location** : direct access to the phone just pressing the emergency button and confirming that you want to make the emergency call to the nearest health center.
- **Voicenotes**: implement the voice notes during the "experience" to have a personal tracking of the different feelings and situations to check them at any time in the history
- **Rating Experience**: when you click on the finish button of the experience track you will get a span where you can make a general rating of the whole experience

<br>


# Client / Frontend

## React Router Routes (React App)

| Path                  | Component             | Permissions                 | Behavior                                                     |
| --------------------- | --------------------- | --------------------------- | ------------------------------------------------------------ |
| `/`                   | HomePage              | public `<Route>`            | Home page                                                    |
| `/signup`             | SignupPage            | anon only  `<AnonRoute>`    | Signup form, link to login, navigate to homepage after signup |
| `/login`              | LoginPage             | anon only `<AnonRoute>`     | Login form, link to signup, navigate to homepage after login |
|                       |                       |                             |                                                              |
| `/experience`         | ExperienceStartPage   | user only `<PrivateRoute>`  | Shows the form to start the new experience                   |
| `/experience/track`   | ExperienceTrackPage   | user only `<PrivateRoute>`  | Tracks and adds a new experience                             |
| `/experience/history` | ExperienceListlPage   | user only `<PrivateRoute>`  | Shows all experiences                                        |
| `/experience/:id`     | ExperienceDetailsPage | user only `<PrivateRoute>`  | Details of the experience                                    |
| `/learn`              | LearnPage             | user only  `<PrivateRoute>` | List of all substances with brief info                       |
| `/learn/:id`          | SubstancePage         | user only `<PrivateRoute>`  | Complete information of the selected substance               |
| `/profile`            | ProfilePage           | user only `<PrivateRoute>`  | Shows user info                                              |
| `/profile/edit`       | ProfileEditPage       | user only  `<PrivateRoute>` | Edit user info or delete account                             |




## Components -----------

- Navbar






## Services ------------

- Auth Service
  - auth.login(user)
  - auth.signup(user)
  - auth.logout()
  - auth.me()




<br>


# Server / Backend


## Models

User model

```javascript
}
  username: {type:String},
  password: { type: String, minlength: 6, required: true }, 
  email: { type: String, match: /^([\w-\.]+@([\w-]+\.)+[\w-]{2,4})?$/, required: true, 		unique: true },
  phonenumber: {type:Number},
  weight: { type: String},
  age: { type: Number, default: 18 }, 
  profilepic: { type: String, default: '/icons/default.png' },
  pathologies: [{type: String}],
  experiences:[{type:Schema.Types.ObjectId, ref: "Experince"}],
}
```



Experience model

```javascript
{
  user: { type: Schema.Types.ObjectId, ref: 'User' , default: null},
  substances:[{type: Schema.Types.ObjectId, ref:'Substance'}],
  duration: {type: Date},
  date: {type: Date},
  memotionStatus:{type:String, enum: ["Much Better", "Better", "Same", "Worse", "Much 		worse"]},
  moodStatus: {type: String, enum: ["Calm", "Energized", "Fearful", "Tense", "Happy", 		"Sad"]},
  eatStatus: {type: String, enum: ["Empty", "Normal", "Full"]},
  intention: {type: String, enum: ["Discover", "Grow", "Fun", "Transform", "Heal"]}
}
```



Substance model

```javascript
{
  name: {type: String},
  type: {type: String},
  description: {type: String},
  information: {type:String},
  dose1: {type: String},
  dose2: {type: String},
  dose3: {type: String},
  maxdose: {type: String},
  mixWith: [{type: String}],
  nonMixWith: [{type: String}]
}
```




## API Endpoints (backend routes) ----------

| HTTP Method | URL                         | Request Body                                                | Success status | Error Status | Description                                                  |
| ----------- | --------------------------- | ----------------------------------------------------------- | -------------- | ------------ | ------------------------------------------------------------ |
| GET         | `/auth/me`                  | Saved<br />Session                                          | 200            | 404          | Check if user is logged in                                   |
| POST        | `/auth/signup`              | {username,<br/>email, <br />password<br />Age<br />}        | 201            | 404          | Checks if fields not empty (422) and user not exists (409), then create user with encrypted password, and store user in session |
| POST        | `/auth/logout`              | (empty)                                                     | 204            | 400          | Logs out the user                                            |
| POST        | `/auth/login`               | {username, <br />password}                                  | 200            | 401          | Checks if fields not empty (422), if user exists (404), and if password matches (404), then stores user in session |
|             |                             |                                                             |                |              |                                                              |
| GET         | `/api/experience`           |                                                             |                |              |                                                              |
| POST        | `/api/experience`           |                                                             |                |              |                                                              |
| GET         | `/api/experience/track/:id` | {id}                                                        | 201            | 400          | Shows the data from the  created experience                  |
| POST        | `/api/experience/track/:id` | {substance,<br/>duration,<br />notes,<br />voicenotes<br/>} | 200            | 400          | Puts the new data of the tracking inside the experience      |
| GET         | `/api/experience/history`   |                                                             |                | 400          | Show all the experiences                                     |
| GET         | `/api/experience/:id`       | {id}                                                        |                |              | Show the details of an experience                            |
| DELETE      | `/api/experience/:id`       | {id}                                                        | 201            | 400          | delete experience                                            |
|             |                             |                                                             |                |              |                                                              |
| GET         | `/api/learn`                |                                                             | 200            | 500          | show all the substances with a brief explanation             |
| GET         | `/api/learn/:id`            | {id}                                                        | 200            | 500          | shows all the info of an specific substance                  |
|             |                             |                                                             |                |              |                                                              |
| GET         | `/api/profile`              | {id}                                                        | 200            | 400          | Gets all the information of the current user                 |
| PUT         | `/api/profile/edit`         | {name,img}                                                  | 201            | 400          | edit all the info of the current user                        |
| DELETE      | `/api/profile/edit`         | {id}                                                        | 200            | 500          | delete the current user account from the database            |


<br>


## Links

### Trello

[Safe-Use Trello](https://trello.com/b/CJ9Yq3qA/m3-project-safeuse) 

### Wireframes

[Safe-Use Miro Wireframes](https://miro.com/app/board/o9J_lcayir4=/)

### Git

The url to your repository and to your deployed project

[Client repository Link](https://github.com/screeeen/project-client)

[Server repository Link](https://github.com/screeeen/project-server)

[Deployed App Link](http://heroku.com)

### Slides









