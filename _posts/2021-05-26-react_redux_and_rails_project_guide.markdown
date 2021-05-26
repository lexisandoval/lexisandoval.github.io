---
layout: post
title:      "React/Redux & Rails Project Guide"
date:       2021-05-26 15:18:15 +0000
permalink:  react_redux_and_rails_project_guide
---


By the end of the week I will have completed my final Flatiron portfolio project. This last project really threw me in a loop and I must say it was by far the most time-consuming and challenging project I have worked on thus far. Learning Redux was quite difficult for me and it took awhile for me to understand the material as there are *many* files involved in a (or at least *my*) Redux project. So, I thought it would be helpful to provide a getting started guide for the frontend and backend as well as a logic flow guide for Redux.

**When creating a Rails API, it is best to follow these steps:**

1. cd to main directory
2. $ rails new name_of_project_backend –api
3. Create repository on GitHub matching name above, then

     $ git init

     $ git add .
		 
     $ git commit -m "Generated project"
		 
     $ git remote add origin https://github.com/username/name_of_project_backend.git
		 
     $ git push -u origin master

4. Decide which models you wish to create and assign attributes/relationships
5. $ rails g scaffold model_name model_attribute ** OR rails g resource model_name ...
6. Uncomment CORS gem in Gemfile
7. Update routes and add namespacing

       namespace :api do 

          namespace :v1 do
		 
	             resources :model 
					
	        end
		 
       end

8. Create new folders in controller folder and move model controller into v1 folder

     • controllers > api > v1 > model_controller.rb

9. Add gem 'jsonapi-serializer' to Gemfile and bundle
10. $ rails g serializer Model
11. Add attributes that you want to show in the API to serializer
12. Update controller to use serializer

     Example for show action:        
		                                      
		def show
		 
         model_json = ModelSerializer.new(@model).serializable_hash.to_json
		 
         render json: model_json 
		 
		end
		
		



**When creating a React app, it is best to follow these steps:**
	
1. cd to main directory
2. $ create-react-app name_of_project
3. Create repository on GitHub matching name above, then

     $ git init
		 
     $ git add .
		 
     $ git commit -m "Generated project"
		 
     $ git remote add origin https://github.com/username/name_of_project.git
		 
     $ git push -u origin master



**When using Redux, it is helpful to follow this flow:**

1. Add component
2. Add reducer
3. Add reducer to store
4. Build action creators
5. Import connect into component
6. Add const mapStateToProps
7. Change export statement to connect




Hope that can help someone! Thanks for everything Flatiron!


