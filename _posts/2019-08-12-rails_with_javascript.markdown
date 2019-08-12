---
layout: post
title:      "Rails with JavaScript "
date:       2019-08-12 12:13:33 +0000
permalink:  rails_with_javascript
---


For our second to last project at Flatiron, we were asked to take our previous Ruby on Rails project and add dynamic elements through JavaScript and a JSON API. JavaScript handles the dynamic behaviors of a website. An example of this might be: a user clicking a button on the page and information being rendered without the page reloading. In order for this to work you have to interact with the Document Object Model, or the DOM. The DOM represents what we see on the browser screen. By selecting specific HTML elements (buttons, ids, classes, etc), we can change what information is displayed. Below is a sample of how I accomplished this in my project. 

I first had to ensure that my application.js file included all of the JS files I wanted to be used. For my project, I only needed two additional files: meal_plan.js and meals.js. Your application.js file might look something like this:

* `//= require jquery
* //= require jquery_ujs
* //= require bootstrap-sprockets
* //= require_tree 
* //= require meal
* //= require meal_plan`

From there, you can choose which file you want to work on first. Most importantly, you need to ensure that you are using the `.ready() ` method which allows our JavaScript code to run as soon as the full page has loaded and the DOM is ready to be manipulated. This method is also a good place to place any of your event listeners. 

```
$(document).ready(function() {
  addMealEventListener();
  listenForFormSubmit();
})
```

You want to next add your Active Model Serializers ( adding this gem to your Gemfile: `gem 'active_model_serializers'`) and think about which controllers you are working with and which views you are changing. Your serializers allow you to choose what attributes will be displayed and this is where you will want to also add your associations. In your controllers, you will want to ensure your controller actions can respond to JSON. 

```
def show
  set_meal
  respond_to do |format|
    format.html { render :show }
    format.json { render json: @meal, status: 200 }
  end
end
```

Back in your js file, you can add an event listener that will respond to an event, such as a click. For this to work correctly, you also need to add `event.preventDefault()` to the element being clicked on or Rails will take over and you will go to your view page. You don't want the page to re-load, which is why you want to prevent the default action. You can then set up your fetch or ajax request to go to a specific URL and grab your data. I used fetch in my project. Once you have the data, you can assign it to a variable and you can then add information to the DOM. Since JSON data cannot be added to the DOM, you will need to change it to a string/HTML and then it can be rendered to the viewer. With a few lines of code and without the page reloading, you're now rendering the information typically found on the show page via JavaScript!

```
function addMealEventListener() {
$('.myMeal').on('click', function(event) {
  const mealId = $(this).data("mealId");
  event.preventDefault();
  fetch(`/meals/${mealId}.json`)
    .then(function(response) {
      return response.json();
    })
      .then(function(data) {
        let mymeal = new Meal(data)
        let myMealHTML = mymeal.mealHTML()
        $(`ul#meal-${mealId}`).append(myMealHTML)
      })
  });
}
```

I really enjoyed this project as it was a way to combine our knowledge of two languages: Ruby and JavaScript along with what we've learned thus far on full stack development. 
