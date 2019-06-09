---
layout: post
title:      "Ruby on Rails"
date:       2019-06-09 22:06:17 +0000
permalink:  ruby_on_rails
---

As a student at Flatiron School, and really any school with a structured learning environment, you experience so many opportunities to learn. You have to learn how to deal with difficult concepts, with data and information that are all new to you, you have to learn how to deal with setbacks and ultimately you "learn how to learn". That is what this school holds as a core focus. With this project, I went through all of the learning experiences. This is what hit home:  
> If the instructor asks you to complete additional features, or you missed a project requirement, treat this as a learning experience. 

Oh, how true this statement was for me on this project. I started my project early, determined not to repeat the same mistakes as previous projects. My plan was to finish a week early. In reality, a week before the project was due, I was pretty much done with my project, getting ready to start working on my blog post and I looked back through the requirements. My heart dropped! I saw that I misunderstood a key component of the project and it would completely require me to change up my models and my associations. I panicked. I went through uncertainity. I was uncertain if I could fix it and if I couldn't fix it, what would that mean for me as a student, would I have to quit? 

 For this project, we were required to "build a complete Ruby on Rails application that manages related data through complex forms and RESTful routes. The goal of the application is to build a Content Management System". We also had to include validations, a scope method, third party user login, user authentication, a nested resource and display error messages. 
 
 Back to my moment of panic. I went through two full days reviewing my project and really trying to figure out how to fix my models and associations. I learned how to create new branches of my project so that I could work out the thoughts in my head without breaking my project and ruining all of my progress. I attended multiple study group sessions, relied on helpful peers and instructors in order to revamp my project. 
 
In the end, going through this setback and uncertainly proved valuable because it's likely to happen on the job as well. You have to have the courage to own up to the fact that you made a mistake and do everything in your power to fix it. That means being vulnerable and asking for help. 

For my project, I created a way for users to track their daily meals. Users can sign up and create an account and each day they can enter in all of their meals: breakfast, lunch, dinner, snacks and dessert. Users will have the option to 'favorite' a meal so that they can start to see trends. Eventually, I would love to add on features that would allow users to track calories and see suggestions on foods to try based on their individual tastes. 

For my project, I have four models:

A User model.

```
class User < ApplicationRecord
  has_secure_password
  has_many :meal_plans
  has_many :meals, through: :meal_plans
  has_many :meal_schedules, through: :meals

  validates :name, :email, presence: true
  validates :email, uniqueness: true

end

```

A MealPlan model.

```
class MealPlan < ApplicationRecord
  belongs_to :user
  has_many :meal_schedules, dependent: :destroy
  has_many :meals, through: :meal_schedules, dependent: :destroy
  
  validates :goal, :description, presence: true
  default_scope -> { order(updated_at: :desc)}

end
```

A Meal model.

```
class Meal < ApplicationRecord
  has_many :meal_schedules
  has_many :meal_plans, through: :meal_schedules

  validates :protein, :vegetable, :side, :day, :beverage, :beverage_ounces, presence: true
  validates_inclusion_of:favorite, in: [true, false]

  scope :favorite, -> { where(favorite: true) unless Meal.all.empty? }
  default_scope -> { order(updated_at: :desc)}


end


```

A MealSchedule model.

```
class MealSchedule < ApplicationRecord
  belongs_to :meal
  belongs_to :meal_plan

  validates :eating_time, :meal_type, presence: true
  default_scope -> { order(updated_at: :desc)}

  accepts_nested_attributes_for :meal, reject_if: :all_blank


  def meal=(attributes)
    self.build_meal(attributes) unless self.meal_id
  end
end 

```

To use my application, a user must first create a meal plan. My thoughts for this is to offer users multiple options based on their dietary restrictions. The choices are from regular, pescatarian, vegetarian, vegan, keto, or gluten free. You can also choose a goal such as weight loss or tone. Next, you enter your meals: entering in your protein, vegetables, sides, beverage, beverage ounces, the day and whether or not this is a favorite meal. Finally, you enter your "meal schedule" which is the type of meal (i.e, breakfast, lunch, dinner) and your eating time (i.e, morning or afternoon). 

I really enjoyed this project and learning about Rails magic! It was challenging, especially getting the model associations correct, and fun because I was able to implement bootstrap and FaceBook log in. 
