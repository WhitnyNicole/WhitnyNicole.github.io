---
layout: post
title:      "Sinatra Fitness Tracker"
date:       2019-04-06 01:39:39 +0000
permalink:  sinatra_fitness_tracker
---


I’ve just completed my second project. For this project, we were instructed to create a Sinatra app using Sinatra-ActiveRecord, Ruby and SQLite3. We also used CSS and HTML. 

For my project, I decided to build a fitness tracker. For me personally, fitness is a big part of my life and since I was younger, I’ve always tried to stay active. As I’ve gotten older, I’ve realized the importance of being able to track these workouts. Tracking can help for a variety of reasons: such as addressing and noticing any health concerns, identifying a change in energy levels, the presence of stress and improved overall wellness, just to a name a few. In creating my app, I hope to be able to provide users with this same capability as they track their own individual workouts. 

Now to get started with my project, I knew what I wanted to create but I wasn’t quite sure on all the pieces that would be involved. I decided to first simply sketch out my ideas. To do so, I used a software diagaram tool that allowed me to visually map out my thoughts which really helped me start the project off with a clear and concise plan. 

We were required to utilize MVC, which stands for Models, Views and Controllers. Right off the bat, I knew that I would have models for users and workouts. And after some thinking, I decided to include a model for exercises as well. After creating my models, I needed to focus on the relationships. I was able to determine that a user would `have_many ` workouts and a workout would `belong_to ` a user. In addition, a workout would `have_many` exercises and an exercise would `belong_to` a workout. The last relationship was that a user would `have_many ` exercises `through `workouts. 

```
class User < ActiveRecord::Base
  has_many :workouts
  has_many :exercises, through: :workouts
  has_secure_password

  validates :name, :email, presence: true
  validates :email, uniqueness: true
end
```

```
class Workout < ActiveRecord::Base
  belongs_to :user
  has_many :exercises
  end
end
```

```
class Exercise < ActiveRecord::Base
  belongs_to :workout
end
```

Next, focusing on CRUD (Create, Read, Update and Destroy), I wanted the ability to have users sign up and create an account and be able to create new workouts and associating exercises. A user also needed to be able to view, edit and delete these items. Using this logic, I created my views, keeping in my mind the seven RESTful routes. The nice thing about this, was that most of the code was similar across the multiple controllers. The controllers consisted of an application controller that inherited from Sinatra::Base and a users controller, workouts controller and exercises controller that all inherited from the application controller. This was important as it allowed me the ability to access ActiveRecord methods across all controllers. 

```
class WorkoutsController < ApplicationController

  get "/workouts" do
    @workouts = current_user.workouts
    erb :"/workouts/index"
  end

  get "/workouts/new" do
    erb :"workouts/new"
  end

  post "/workouts" do
    redirect_if_not_logged_in
    if params[:category] && params[:duration] != ""
      @workout = Workout.create(category: params[:category], duration: params[:duration], user_id: current_user.id)
      flash[:message] = "Congrats, you entered a new workout category!" if @workout.id
      redirect "/workouts/#{@workout.id}"
    else
      flash[:errors] = "Oops, that workout could not be created."
      redirect 'workouts/new'
    end
  end
```

With those ideas in mind, I used the corneal gem to create the framework for my project. This was a huge time saver. I also used a variety of resources to assist with my code: namely Flatiron’s live lectures, project office hours, our written class materials and labs and multiple websites. It took me a full week to get everything coded out and then another few days of refactoring. For me, refactoring was tough because once I got something working, I was always afraid that it would break with any changes! My application did break, quite a few times I might add, but I was able to review the error messages and mostly understand what needed to be fixed. I actually think this part helped me to better understand my code and what I was trying to accomplish. One side note, this was also my first time coding locally and I really enjoyed the process. 

Overall, I would like to share a few things that were helpful to me during this process:
1. First and foremost, having a solid plan and project idea was important as it was a guide for the entire process 
2. Next, reviewing my project on a daily basis was important because it allowed me to keep track of what was working well and what needed to be tweaked. For instance, several days into my project, i decided that my workouts controller and exercises controller needed some additional columns and I needed to create a few new migrations. 
3. Running my application multiple times also helped to ensure the overall project was continuing to flow as I designed and helped me while I was coding out my routes
4. The last thing that I found helped was talking through my code out loud and sharing ideas with my classmates and instructors 

Overall, I really enjoyed this process and I am excited about what I created. I would love for you to try out my app and I sincerely hope that it helps you as much as it’s helped me!
