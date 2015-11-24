
# Forms And Basic Associations Rails

## Objectives

1. Populate select options based on association options.
2. Assign a FK based on a select box value directly through mass assignment. (post[category_id])
3. Define a belongs_to association writer.
4. Build a form field that will delegate to a belongs_to association writer. (post#category_name=) through controller mass assignment.
5. Define a has_many association writer.
6. Build a form field that will delegate to a has_many association writer. (owner#pet_names=) through controller mass assignment.

## Notes

were explicitly not teaching nested_attributes or nested ar forms right now.

This is about showing the student the power of custom writers that can handle complex association logic but still work with mass assignment.

Start with assigning a post a category (we'll pre-seed their development.db with categories). This lab can include all the code required - so all the models and controllers and views can be fully wired and we're just showing them how to update the post#new and edit form to assign a category. we're going to have an unrelated domain of owners and pets within this rails app too because I can't thing of a reasonable has_many/belongs_to for posts.

Have them build an input text field called post[category_id] where the user could enter the category pk and category_id will be correctly mass assigned and the post will have that category. that's the basic association assignment.

point out that users would never do that as they don't know what a PK is - we need to give them the name of the category id but assign a pk. let's build a select input - we'll use basic html first <select>Category.all.each do |c| <option> end type thing so they can see all the details and see how a select box passed the selected options' value.

Then we can show them the collection_select form helper.

That works great but what about if you wanted to create a new category and not an existing one?

all that's happening to assign the category is that our form is writing to category_id= which is provided by AR. But what if we just built our own writer called category_name=. What would that method need to do? take a string, find the category and assign it. we can also extend the functionality of this method to find or create a category by name. by building the method we keep our controller action super clean and bring all the logic into our model.

let's look at a has_many association in a different domain. imagine an owner having many pets where every pet belonged to an owner. How might we allow a user to input many pets for an owner when editing or creating an owner?

The pet and the owner doesn't exist yet so we have no existing rows in the database to link with foreign keys.

let's imagine the form having a text field for each pet we want to add where the value of the text field will be the pets name.

we could have <input type="text" name="owner[pets_name_1]"> and then try to iterate and collect all the pet names from params but that wouldn't be great. is there a way to get params to hold an array of data?

<input type="text" name="owner[pets_names][]"> with that array field that will group all the pet names they enter into a single param key that points to an array of string pet names.

then we just need to build pets_names= to be able to mass assign.

what does that method need to do? iterate and build a pet for each name and assign that pet to the owner

def pets_names=(names)
  names.each do |name|
    self.pets.build(:name => name)
  end
end

once they build that writer the form just works
also show that we could add an arbitrary amount of pets this way so the form itself mirrors the many nature of the relationship

we're also setting up nicely to move to nested attributes for a has many using this sort of custom writer.
