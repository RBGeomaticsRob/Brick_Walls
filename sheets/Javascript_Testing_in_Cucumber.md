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