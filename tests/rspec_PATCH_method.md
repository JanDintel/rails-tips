# Using Rspec with Rails 4 patch method
In Rails 4 PATCH is the new HTTP methode for an update action. You can find the details here: http://weblog.rubyonrails.org/2012/2/25/edge-rails-patch-is-the-new-primary-http-method-for-updates/

## Using PATCH in your specs
As you can imagine you need to use the PATCH method to test the update action in your controller. Because of how the users controller in this example works you need to specify the id and user. This particular spec tests whether the user gets redirected if it's not logged in.

(./spec/controllers/users_controller_spec.rb)
```ruby
require 'spec_helper'

describe UsersController do
  let(:user) { FactoryGirl.create(:user) }

  describe "#update" do
    before { patch :update, id: user, user: user }
    specify { expect(response).to redirect_to(sign_in_path) }
    it { should_not have_selector('div.alert.alert-notice', text: 'Need to be logged in') }
  end
end
```

## Users controller with update action and strong paramaters
The update action in the users controllers uses the new strong parameters of Rails 4 and needs to find a user by his id first. That's why the id and user are specified in the spec.

(./app/controllers/users_controller.rb)
```ruby
# Update action
def update
  @user = User.find(params[:id])
  if @user.update_attributes(user_params)
    flash[:success] = "Updated account"
    sign_in @user # Session token gets reset after update, needs to set again
    redirect_to @user
  else
    render 'edit'
  end
end
```

```
# Strong parameters
private
  def user_params
    params.require(:user).permit(:name, :email, :password, :password_confirmation)    
  end
```

The tests should pass now.


