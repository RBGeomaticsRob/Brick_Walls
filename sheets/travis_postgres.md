#Travis with a Sinatra Postgres Project#

Where a project relates to a postgres database, you will need to inform Travis how to create this database for testing purposes.

This can be done in the `.travis.yml` file of your project. You will need to add a before installation section to describe which flavour of postgres you would like setup on travis. A 'before script' to run a psql command to setup the database for you. So you will have a travis file looking something like the following:

```yml
rvm: '2.2.1'

before_install:
  - sudo apt-get update -qq
  - sudo apt-get install -qq postgresql-server-dev-9.4

before_script:
  - psql -c 'create database chitter_test;' -U postgres

script:
  - bundle exec cucumber
  - bundle exec rspec spec
```

If you are also running rake tasks for transferring the schema to the database you will also need to add a script to set this up, if this is the case your .travis.yml should look something like this:

```yml
rvm: '2.2.1'

before_install:
  - sudo apt-get update -qq
  - sudo apt-get install -qq postgresql-server-dev-9.4

before_script:
  - psql -c 'create database chitter_test;' -U postgres

script:
  - RACK_ENV=test bundle exec rake auto_upgrade
  - bundle exec cucumber
  - bundle exec rspec spec
  ```

  ##Coveralls Problem##

  This may be unrelated, but when making these changes, I also then had a problem with coveralls, producing a 422 error and a message 'Couldn't find a repository matching this job.'. I solved this by adding a .coveralls.yml file with a repo_token in it from the coveralls site as follows:

```yml
repo_token: FNRW73BFNOGGOJEP94UBTBGJRWJY48839
```
