# Quiz app

### Clone this repository
```
git clone git@github.com:llinards/vuejs-quiz-app.git
```

### Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

## Public demo
See [here](https://romantic-beaver-f72573.netlify.app/).

## Description:

This is a front-end client built using VueJS 3. The template was created using Vue CLI because it provides
a very easy starting point to create projects without much hassle. Useful scripts are already created for
hot-reload, production, and development builds. Vue JS was selected as a language because it's a great framework
to start work on a project without spending too much time setting it up. Its concept is a "single-file component" so you can easily maintain three main parts - user interface (HTML), logic (JavaScript), and style (CSS). Other frameworks would make this relatively easy solution much harder to read, expand, maintain. The only NPM package installed is Axios to make API requests much easier.

The logic behind the app is quite simple. At the first screen, a user is presented with two input elements. Name and dropdown to select quiz. When this component is called/created, an API request has been made to get all available quizzes and stored in the array. The user must provide information for both inputs otherwise it will fail and return an error message. After that main component where all interactions happen is called. With the previously selected quiz, its ID has been saved as the property and passed to the next component where another API request is made to get all questions in the quiz. All of these questions are saved in the array and looped through when a quiz is in progress. Answers are fetched every time user submits them. This request accepts an array of answers and returns correct answers with the total amount. The correct answer amount is saved in the local property which in the end is being passed to the last component where the user gets a result of the quiz. If a user wants to try the quiz again, the button reloads the whole app.

For easier state management Vuex could be used but wanted to keep as simple as possible and deliver an MVP version regarding the provided data API.