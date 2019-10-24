---
layout: post
title:      "My Final Project"
date:       2019-10-13 15:14:21 -0400
permalink:  my_final_project
---


For my final project, I really wanted to create a job search application that could help me keep track of my upcoming interviews. I also wanted the ability to keep track of the questions being asked at each interview as a way to help me better prepare for future interviews. 

For this project we were asked to create a Rails API backend and a frontend using react and redux. The backend was pretty straightforward to get set up. I had three models: user, interview and review. Although I didn't use the user model for my project, I hope to be able to build that out in the future. For my assocations, an interview `has_many` reviews and a review `belongs_to` to an interview. I also utilized Active Model Serializers. 

For me understanding the flow of how react and redux works was the tricky part. 

Here are the steps that helped me gain a better understanding of the flow, using one example:

1. When a user visits my interview index page: `"/interviews`, my `InterviewList` container component will render
2. As soon as this component mounts, it will kick off the `getInterviews` action which will then make a call to my rails API
3. With a successful API call, you dispatch to the reducers, then the` InterviewsReducer` checks the `action.type` which then runs through the various cases in the switch statement. Once it hits the case that matches the type then the code for that case will execute and we will get access to the `action.payload` and our case will return the payload and will update our state
