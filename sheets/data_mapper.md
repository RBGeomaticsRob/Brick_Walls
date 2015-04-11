#Data Mapper#

These examples have heavily leant on the excellent Data Mapper [documentation](http://datamapper.org/docs/) and hence are mainly my paraphrasing and interpretation of this.

##Setup server##

Requires the `data_mapper` gem

Ensure a database has been created in postgres before trying to make a connection

(in the server.rb or similar file before declaring the server class)

*Optional, displays the log in the terminal*
DataMapper::Logger.new($stdout, :debug)

*Establish a Postgres connection*
`DataMapper.setup(:default, 'postgres://localhost/database_name')`

*Require in the models (ruby table descriptions)*
`require './lib/user'`

*Finalizing the connection, this checks the validity of the model and relationships and needs to be called before trying to access the models*
`DataMapper.finalize`

*Will build or extend the current schema to the model, but will not overwrite, to overwrite change to `auto_migrate!` but be careful as this will wipe the data each time if left.*
`DataMapper.auto_upgrade!`

##Setup the Models##
Table structures are called Models and can be set up as ruby classes in the lib directory, with the DataMapper::Resource module included.
```ruby
class User
  include DataMapper::Resource

  property :id,         Serial
  property :name,       String
  property :email,      String
  property :age,        String
  property :address,    Text
end
```
Every model *must* have a primary key
The following are available as data types from the core library (can be extended to more with dm-types)
- Boolean
- String (default length limit of 50 characters)
- Text (defaults to lazy loading and length limit of 65535 characters)
- Float
- Integer
- Decimal
- DateTime, Date, Time
- Object, (marshalled)
- Discriminator
- Binary (inherits default length limit of 50 characters from String)

Other settings
Require a Field `property :name,     String, required: true`
Composite Key - make a primary key from more than one property
```ruby
property :old_id, Integer, key: true
property :new_id, Integer, key: true
```
Default value `property :age,     String, default: 30`
it can also take a lamda `property :md5sum, String, default: lambda { |r, p| Digest::MD5.hexdigest(r.path.read) if r.path }`
Limiting Access `property :email, String, accessor: :private`

##Assciations##

Associations describe the relationships between Models and are defined in the model classes.

###One to Many###
The example below re-opens these classes but they would normally be declared directly in the model class.

```ruby
class User
  has n, :support_calls
end

class Support_Calls
  belongs_to :user
end
```
In this example user has a 1 to many relationship with support calls. A user can have many support calls, but a support call can only have one user.

###One to Many Through###
This allows you to create a 'link table' to allow a one to many relation ship from a class to the link table and in turn by associating two classes to the 'link table' hence establishing a many to many relationship in a Model.

```ruby
class Photo
  include DataMapper::Resource
    property :id, Serial

    has n, :taggings
    has n, :tags, through: :taggings
  end

class Tag
  include DataMapper::Resource
    property :id, Serial

    has n, :taggings
    has n, :photos, through: :taggings
  end

  class Tagging
    include DataMapper::Resource

    belongs_to :tag,   :key => true
    belongs_to :photo, :key => true
  end
```

###Many to Many###
Uses an anonymous resource to link two models together in a many-to-many relationship.
```ruby
class Article
  include DataMapper::Resource

  property :id, Serial

  has n, :categories, :through => Resource
end

class Category
  include DataMapper::Resource

  property :id, Serial

  has n, :articles, :through => Resource
end
```

##Adding records to a table###
```ruby
User.create(name: 'Holly',
            email: 'test@mail.com',
            age: 35,
            address: 'My Address')
```

*Note the id field will self generate with the next number in series as it was set to 'Serial'.*

###Extracting Datamapper setup into a helper###

It may be useful to extract the datamapper configuration into a helper to add to the readability of code in your sinatra server file and to allow automation of tasks with rake. This can be done as follows, this asumes that you extract to a file called `data_mapper_setup.rb` in the root of your app.

In your sinatra server file

```ruby
require_relative 'data_mapper_setup'
```

In data_mapper_setup.rb

```ruby
# This line assumes you have set ENV['RACK_ENV'] = 'test' in your rspec(spec_helper.rb) or cuke(env.rb).
# And that you have appended your database names is _test and _development for test and development versions.
env = ENV['RACK_ENV'] || development

DataMapper.setup(:default, "postgres://localhost/your_database_name_#{env}")
# require all your model files here
require '.app/models/user'
require '.app/models/items'
DataMapper.finalize
```

You will have noticed that auto_upgrade! or auto_migrate! have not been included in this helper, that is because we don't necessarily want to update the schema everytime we run the app, as this often will not change. We wan't to be able to update the schema as we are developing though, so we can add this functionality in the Rakefile as follows:

```rake
require 'data_mapper'

task :auto_upgrade do
  require './app/data_mapper_setup'
  DataMapper.auto_upgrade!
  puts 'Auto-upgrade complete (no data loss)'
end

task :auto_migrate do
  require './app/data_mapper_setup'
  DataMapper.auto_upgrade!
  puts 'Auto-upgrade complete (data could have been lost)'
end
```

Now you can run `rake auto_upgrade` to do additional upgrade to the schema from the models or `rake auto_migrate` to do destructive updates to the schema from the models.