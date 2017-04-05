# Generate models with foreign key
    $ rails generate model photo album:references

This command will generate photos table with integer field album_id and also it will add index for this field automatically. Make sure in it by looking at generated migration:

    class CreatePhotos < ActiveRecord::Migration
      def change
        create_table :photos do |t|
          t.references :album

          t.timestamps
        end
        add_index :photos, :album_id
      end
    end

# Many to many relations:
An example is the relation about offers and tags this is the best way to build the relation:
```
class CreateOffers < ActiveRecord::Migration[5.0]
  def change
    create_table :offers do |t|
      t.string :title
      t.string :description
      t.attachment :photo
      t.string :address
      t.boolean :outstanding
      t.boolean :is_active, default: true

      t.timestamps
    end
  end
end
```

```
class CreateTags < ActiveRecord::Migration[5.0]
  def change
    create_table :tags do |t|
      t.string :name

      t.timestamps
    end
  end
end
```
```
class CreateOfferTags < ActiveRecord::Migration[5.0]
  def change
    create_table :offer_tags do |t|
      t.references :offer, foreign_key: true
      t.references :tag, foreign_key: true

      t.timestamps
    end
  end
end
```
**In models:**
```
class Offer < ApplicationRecord
  has_many :offer_tag
  has_many :tag, through: :offer_tag
end
class OfferTag < ApplicationRecord
  belongs_to :offer
  belongs_to :tag
end
class Tag < ApplicationRecord
	has_many :offer_tag
	has_many :offer, through: :offer_tag
	validates :name, presence: true
	validates :name, uniqueness: true
end
```
And we have builded our many to many relation.

# A good explaination about Lambda and Procs
Next explanation some cases about 
[lambda and procs objects](http://augustl.com/blog/2008/procs_blocks_and_anonymous_functions/)

```
say_hello = Proc.new { puts "Hello!" }
say_hello.call
# => "Hello!"


a_lambda = lambda {|a| a.inspect }
puts a_lambda.call
# => nil
puts a_lambda.call("foo", 5)
# => ["foo", 5]
```

# Runnig tasks or workers:
[Here](https://github.com/resque/resque-scheduler) is a good tutorial of how to make rails workers. 
Run with the command: ``` rake environment resque:scheduler ```.

[Another option](https://github.com/resque/resque) is it.

# Tests
1. [Shoulda Matchers](https://github.com/thoughtbot/shoulda-matchers)
2. [Factories](https://github.com/thoughtbot/factory_girl/blob/master/GETTING_STARTED.md)
3. [Faker for Factories](https://github.com/stympy/faker)
4. [Controller Examples to test](http://codecrate.com/2014/11/rspec-controllers-best-practices.html)
5. [Controller's specs](https://everydayrails.com/2012/04/07/testing-series-rspec-controllers.html)

## Example of controller test:
If i have a method called create, but in routes is called in diferent form, I need to call to the method with the create If I am in controller spec, but if I am in request spec, I need to call the method with the name of the route.

```
...
    before :each do
      controller.stub(:authenticate_user!).and_return(true)
      controller.stub(:check_permissions).and_return(true)
      controller.stub(:current_user).and_return(operator)
    end
    it "should return 200 and the auth token" do
      operator.confirm!
      operator.stub(:ensure_authentication_token!).and_return("token")
      post :create, {'email'=>operator.email, 'password'=>'123456'}, format: :json
      json_body = JSON.parse(response.body)
      json_body['token'].should == @operator.authentication_token
    end
```
