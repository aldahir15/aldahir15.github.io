---
layout: post
title: Calorie-Calculator
---
[Project Page](https://github.com/aldahir15/CalorieCalculator) <br />
Long time, no see. Sorry for the lack of updates on this blog, studying got the best of me.
Let's get into it. I want to show you guys a recent project I just completed, it's essentially a calorie calculator based on several metrics in order to get a calorie target to lose x amount of weight per month.
(PS. I hate CSS lol)
I knew that it wouldn't be too hard of a application to pursue considering i'm pretty knowledgeable about fitness (not to toot my own horn). The parameters taken into account in this calculator were:
* Age
* Height (cm)
* Weight
* Sex
* Body Fat %
* Activity Level
* Pounds to lose (per month)
I created a user model to take in these parameters, making sure that age, height, weight, and body fat were formatted within a valid regex
~~~~
class User < ApplicationRecord
  VALIDATES_REGEX = /\A[+-]?\d+\z/
  validates :age, presence: true, format: { with: VALIDATES_REGEX }
  validates :height, presence: true, format: { with: VALIDATES_REGEX }
  validates :weight, presence: true, format: { with: VALIDATES_REGEX }
  validates :bodyfat, presence: true, format: { with: VALIDATES_REGEX }
end
~~~~
After creating my User model, I created a Users controller with three simple actions:
* new
* create
* show
I also created a private user params action to refer back to from create.
~~~~
class UsersController < ApplicationController

  def new
    @user = User.new
  end

  def show
    @user = User.find(params[:id])
  end

  def create
    @user = User.new(user_params)
    if @user.save
      redirect_to @user
    else
      render 'new'
    end
  end



  private

  def user_params
    params.require(:user).permit(:age,:height,:weight,:sex,
                                  :bodyfat, :activitylevel, :pundstolose)
  end
end
~~~~
Now we're able to start creating our views, it's actually pretty simple since we only need views for new and show.
I opted on using Bootstrap for this since it makes everything much easier. I also ended up creating a custom.scss file akin to what i've been learning on Micheal Hartl's Ruby on Rails' Tutorial for custom formatting.
Before starting on our 'new' view I wanted to have a header and footer on this application so I ended up making partials for them and calling them from my application.html.erb layout file.

Now we're finally able to get started on our 'new' view.
I ended up wrapping the whole contents of new into a div to allow the forms to be centered.
I created text fields for: age, height, weight, and body fat. Then for sex, activity level, and pounds to lose I simply used select to be able to chose from a drop down.

~~~~
<div class="col-md-6 col-md-offset-3">
  <%= form_for @user do |f| %>
    <br>
    <%= f.label :age %><br>
    <%= f.text_field :age, class: 'form-control' %><br>
    <%= f.label :height, "Height in cm" %><br>
    <%= f.text_field :height, class: 'form-control' %><br>
    <%= f.label :weight %><br>
    <%= f.text_field :weight, class: 'form-control' %><br>
    <%= f.label :sex %><br>
    <%= f.select :sex, ['Male','Female'], class: 'form-control' %><br>
    <br>
    <%= f.label :bodyfat, "Body Fat %" %><br>
    <%= f.text_field :bodyfat, class: 'form-control' %><br>
    <%= f.label :activitylevel, "Activity Level" %><br>
    <%= f.select :activitylevel, ['1 (Desk job with little exercise)', '2 (1-3 hrs/wk of light exercise)','3 (3-5 hrs/wk of moderate exercise)','4 (5-6 hrs/wk of strenuous exercise)','5 (7-9 hrs/wk of strenuous exercise)'], class: 'form-control'%><br>
    <br>
    <%= f.label :pundstolose, "Pounds to Lose per month" %><br>
    <%= f.select :pundstolose, ['1', '2','3','4','5','6','7','8'], class: 'form-control' %><br>
    <br>
    <%= f.submit "Submit", class: "btn btn-primary"%>
  <% end %>
  <br>
</div>
~~~~

Now when we click submit, our 'create' action adds our user to our database and can be referenced as '/user/#'. But obviously we aren't able to see this page yet since we haven't added the 'show.html.erb' view yet.
For 'show' once again I wrapped the contents in a div to center them. Then we call all the parameters from that user to display, then we use some embedded ruby to do several calculations to get our BMI, BMR, TDEE, and lastly the calories we need to eat to lose x pounds per month.

~~~~
<br>
<div class= "containter text-center">
  <div class="col-lg-8 col-lg-offset-4">
    <div class="bigJumbo">
      Age: <%= @user.age %><br>
      <br>
      Weight: <%= @user.weight%><br>
      <% weight_in_kg = @user.weight.to_i * 0.45359237 %>
      <br>
      Height: <%= @user.height%><br>
      <% weight_in_in = @user.height.to_i * 0.39 %>
      <br>
      Sex: <%= @user.sex%><br>
      <br>
      Activity Level: <%= @user.activitylevel%><br>
      <br>
      Body Fat: <%= @user.bodyfat%><br>
      <br>
      Pounds to lose per month: <%= @user.pundstolose%><br>
      <br>
      <div class="customJumbo">
        <br>
        <% firstBMI = (@user.weight.to_i * 0.45)%>
        <% secondBMI = (weight_in_in * 0.025) ** 2%>
        BMI: <%= (firstBMI / secondBMI).round(2) %>
        <br>
        BMR:
          <% if @user.sex[0] == "M" %>
            <%= bmr = ((9.99 * weight_in_kg) + (6.25 * @user.height.to_i) - (4.92 * @user.age.to_i) + 5).to_i %>
          <% else %>
            <%= bmr = ((9.99 * weight_in_kg) + (6.25 * @user.height.to_i) - (4.92 * @user.age.to_i) - 161).to_i %>
          <% end %>
        <br>
        <% if @user.activitylevel[0] == '1' %>
          <% maintenancefactor = bmr * 1.2 %>
        <% elsif @user.activitylevel[0] == '2' %>
          <% maintenancefactor = bmr * 1.375 %>
        <% elsif @user.activitylevel[0] == '3' %>
          <% maintenancefactor = bmr * 1.55 %>
        <% elsif @user.activitylevel[0] == '4' %>
          <% maintenancefactor = bmr * 1.725 %>
        <% elsif @user.activitylevel[0] == '5' %>
          <% maintenancefactor = bmr * 1.9 %>
        <% end %>

        TDEE: <%= tdee = maintenancefactor.to_i %>
        <br>
        <% one_pound = 3500 %>
        <% subtractcals = ((@user.pundstolose.to_i * one_pound) / 30) %>
        <h4>
          Based on your Acitvity Level and your monthly goal and you should eat
          <%= (tdee - subtractcals).to_i %> calories a day.
        </h4>
        <br>
      </div>
    </div>
  </div>
</div>
~~~~

This is all the calculator can do currently, but I want to add the ability to log in so that you can save your information and reference it later on, also have a graph function so that every time you update your information, you can see an interactive view of how your weight loss is going.
I'll keep you guys/gals updated with my progress!
