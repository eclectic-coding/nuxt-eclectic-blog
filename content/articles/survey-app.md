---
title: Survey App for Flatiron Project
date: 2020-01-17
published: true
tags: ['Coding','JavaScript','Fullstack']
series: false
cover_image: images/survey-app.jpg
canonical_url: false
description: This was an exciting project pulling together all that I have been learning at Flatiron School over the last four months. I have come to appreciate the simplicity of setting up an API with Rails.
---
### Introduction

#### Requirements
This was an exciting project as it pulled together all that I have been learning at [Flatiron School](https://flatironschool.com/) over the last four months. I have come to appreciate the simplicity of setting up an API with Rails. The project requirements were as follows:
- Build a Single Page Application (SPA), with the frontend built with HTML, CSS, and JavaScript and a backend API with [Ruby and Rails](https://rubyonrails.org/).
- Communication between the front and backend using asynchronously (AJAX) communication and JSON as the communication format.
- The JavaScript application must use Object Oriented JavaScript (classes) to encapsulate related data and behavior.
- The domain model served by the Rails backend must include a resource with at least one has-many relationship.
- The backend and frontend must collaborate to demonstrate Client-Server Communication. Your application should have at least 3 AJAX calls, covering at least 2 of Create, Read, Update, and Delete (CRUD). Your client-side JavaScript code must use fetch with the appropriate HTTP verb, and your Rails API should use RESTful conventions.

### App Design
**HTML** - the footprint starts with a HTML file loosely based on HTML5 Boilerplate project, with a few of my own modification. I prefer to group the folder structure to *separate concerns* so, the source files are grouped into a `src` which includes separate folders for `js`, `styling`, and `images`. The compiled and minified files for production, are grouped into a 'dist' folder structure, again separated by `js`, `styling`, and `images`.

**Styling** - Most projects I have spun put pretty quickly and have relied on Component UI's to decrease the development time. I have used [Bootstrap](https://getbootstrap.com/), and [TailwindCSS](https://tailwindcss.com/) in the past. This site is built with [Bulma](https://bulma.io/), which I love.
- SCSS Boilerplate I customized based on original work by [Hugo Giraudel](https://hugogiraudel.com/) and his [SASS_Boilerplate](https://github.com/HugoGiraudel/sass-boilerplate)
- Styling is formatted, compiled, and minified using Gulp, and Browersync. The Gulp file is my tweaks to a [Gulp-Boilerplate](https://github.com/cferdinandi/gulp-boilerplate) originally designed by [Chris Ferdinandi](http://gomakethings.com/)
- The App allows users to create, delete, and complete (update) surveys, which then will display a running result (*this is not the best design, as a admin account needs to added to handle delete, but this meets the requirements of the project*). It has been very satisfy to code the styling for this project.

**API** - the API is changed with Ruby on Rails in API Mode utilizing a [Postgres](https://www.postgresql.org/) database. There are two database tables: 1) Surveys to save each survey list and three questions, and 2) an Answers table which saves the survey responses and the corresponding `survey_id`.

### Fetch API
To set up the index page when I user visits the site, I used a simple `GET` request using the Fetch API. It is with this design I encountered a bug and an opportunity for learning. The following fetch call was at the head of the `index.js` file.
```javascript
  fetch('http://localhost:3000/surveys')
    .then(res => res.json())
    .then(surveys => {
      surveys.forEach(survey => {
        const { id, title, question1, question2, question3 } = survey
        new Survey(id, title, question1, question2, question3)
      })
    })
```

When the user visited a single Survey page and clicked delete, the survey was in fact deleted, but it required a manual refresh to bring back the index display. I refactored the root fetch call:
```javascript
function fetchSurveys() {
  fetch('http://localhost:3000/surveys')
    .then(res => res.json())
    .then(surveys => {
      surveys.forEach(survey => {
        const { id, title, question1, question2, question3 } = survey
        new Survey(id, title, question1, question2, question3)
      })
    })
}
fetchSurveys()
```
This refactor meant in the `deleteSurvey` method in the `Survey` class I could call this function to display the Surveys again:
```javascript
async deleteSurvey() {
    await fetch(`http://localhost:3000/surveys/${ this.id }`, {
      method: 'DELETE'
    })
      .then(() => {
        document.getElementById('survey-container')
          .removeChild(document.getElementById(this.id))
      })
    fetchSurveys()
  }
  ```

### Promise Pretty Please?
The next lesson I learned in this project was that a **Promise** is *NOT* the same as **DATA**. I struggled when I realized I could not really create a "*global variable*" to use throughout the project. I ended up utilizing JavaScript to Manipulate the [Document Object Model](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction#DOM_and_JavaScript) to interject the results of the survey. I would love to abstract this code but it works:

```javascript
getResults() {
    const fetchPromise = fetch('http://localhost:3000/answers')
    const resultsReport1 = document.getElementById('q1')
    const resultsReport2 = document.getElementById('q2')
    const resultsReport3 = document.getElementById('q3')
    fetchPromise.then(resp => {
      return resp.json()
    }).then(questionResults => {
      const myResults1 = questionResults.filter(a => a.surveys_id && a.responded === 'question1').length
      resultsReport1.innerHTML += myResults1
      const myResults2 = questionResults.filter(a => a.surveys_id && a.responded === 'question2').length
      resultsReport2.innerHTML += myResults2
      const myResults3 = questionResults.filter(a => a.surveys_id && a.responded === 'question3').length
      resultsReport3.innerHTML += myResults3
    })
  }
```
Which manipulates the DOM based on this template:
```javascript
resultsHTML() {
    return `
    <div id="results-card">
      <h3>Results:</h3>
        <ul class="report-list">
          <li>${ this.question1 }: <span id="q1"></span></li>
          <li>${ this.question2 }: <span id="q2"></span></li>
          <li>${ this.question3 }: <span id="q3"></span></li>
        </ul>
     </div>
     <button class="card__btn done">Done</button>
    `
  }
```
Overall, this has been a great learning experience of a Single Page Application and there is plenty of room for future upgrades. Are you interested? Go look at the repo for [future features](https://github.com/eclectic-coding/js-survey-app_frontend/blob/master/Survey-App.md).
