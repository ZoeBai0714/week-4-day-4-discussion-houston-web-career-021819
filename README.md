# Rails Intro Review

Take 30 minutes to discuss the following questions with your table group.

1. Generate a new rails app called 'Rooty Tooty Blendy Fruity'.
2. Generate two models, 'Smoothie' and 'Ingredients', using the *resource* generator.  
  * If a smoothie has ingredients, what sort of Active Record association should these two models have?
  * Both Smoothies and Ingredients should have name column with a String datatype, but what else should be included to set up the appropriate foreign-key relationship? Try to generate these resources without having to modify the migration files afterward. **BONUS:** What datatype could be used instead of :integer to generate a relationship between these models?

rails g resource Smoothie name:string --no-test-framework
rails g resource Ingredients name:string, smoothie_id:integer --no-test-framework

3. Update/Write any needed ActiveRecord associations in the two models that were generated.
class  Smoothie < ActiveRecord::Migration
  has_many :ingredients
end

class  Ingredients < ActiveRecord::Migration
  belongs_to :smoothie
end

4. Discuss amongst your table how you might do the following from here:

  * If you were to implement fully RESTful Smoothie controller, what methods would be needed?
    CRUD(new, create, index, show, edit, update, destroy)

  * What views would be needed for this to work?
     index, show, new, edit
  
  * How would you limit the route resource in config/routes.rb for Ingredients so it will only route to index and create?
     resources :smoothies, only:[:index, :create]

  * Say we wanted to display a specific Smoothie using the show method and include the ingredients that belong to it within the view, what would be needed in the method to display both the Smoothie info and it's related ingredients?  For instance:
 
  **"Green Mango Fusion"**
  
    * 1 mango
    * 1 banana
    * 1 cup frozen berries
    * 1 bunch of kale
    * 2 cups milk


    ## SmoothiesController
    def show
    @smoothie = Smoothie.find(params[:id]) 
    @ingredients = Ingredient.all.select {|ingredient| ingredient.smoothie_id = params[:id]}
    end

    ## smoothies/show.html.erb
    <h1><%= @smoothie.name%></h1>
    <h1>Ingredients</h1>
    <ul>
     <% @ingredients.each do |ingredient| %>
       <li> <%= ingredient.name %> </li>
     <% end %> 
    </ul>