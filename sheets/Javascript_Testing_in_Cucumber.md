Add gem 'selenium-webdriver' to Gemfile

Tag cucumber scenarios on the line above with `@javascript`
```cucumber
  @javascript
  Scenario: User cannot signup when password is not supplied
    Given I am on user signup page
    And I fill in "username" with "citizen1"
    And I fill in "email" with "angry@citizen.com"
    And I fill in "password_confirmation" with "ra88it"
    And I press "Register"
    Then I should see "Sorry, there was something wrong with your password!"
    And I should not see "Welcome, citizen1!"
    And I should be on user signup page
```

This will work if you have firefox installed.

To change to chrome:

In cucumber env setting insert the following after requiring 'capybara' and before giving capybara the app.

```ruby
Capybara.register_driver :chrome do |app|
  Capybara::Selenium::Driver.new(app, browser: :chrome)
end
Capybara.javascript_driver = :chrome
```

To change to webkit (for true headless testing):

`brew install qt`

There is a good tutorial for this [here](https://github.com/thoughtbot/capybara-webkit/wiki/Installing-Qt-and-compiling-capybara-webkit)

Add `capybara-webkit` to the gem file

In cucumber env setting add `require 'capybara-webkit'` insert the following after requiring 'capybara' and before giving capybara the app.

```ruby
Capybara.javascript_driver = :webkit
```


[Source Link](http://collectiveidea.com/blog/archives/2011/09/27/use-chrome-with-cucumber-capybara/)
[Installing Capybara Webkit](https://github.com/thoughtbot/capybara-webkit/wiki/Installing-Qt-and-compiling-capybara-webkit)
[Capybara Webkit](https://github.com/thoughtbot/capybara-webkit)
[Qt download required for Capybara Webkit]()