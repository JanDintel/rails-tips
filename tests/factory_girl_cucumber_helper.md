# Using Factory Girl with a helper for Cucumber

## Using Factory Girl directly in your steps
(./features/step_definitions/function_name_steps.rb)

```gherkin
Given /^the user has an account$/ do
    @user = FactoryGirl.create(:user)
end
```

## Using Factory Girl with a helper
### Create helper
(./features/support/helper_name_helpers.rb)

```ruby
module HelperNameHelpers
    def create_user
        @user = FactoryGirl.create(:user)
    end
end
# Tell Cucumber to include the module into each World object for each Scenario
World(HelperNameHelpers)
```

### Refactor step

```cucumber
Given /^the user has an account$/ do
  create_user
end
```

Test should pass now
