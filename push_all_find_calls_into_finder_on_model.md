## bad
```ruby
<ul>
  <% User.find(order: :last_name).each do |user| %>
    <li><%= "#{user.last_name} #{user.first_name}" %></li>
  <% end %>
</ul>
```
## solutions
### put code into Controller
```ruby
# controller
class UsersController < ApplicationController
  def index
    @users = User.order :last_name
  end
end

# views
<ul>
  <% @users.each do |user| %>
    <li><%= "#{user.last_name} #{user.first_name}" %></li>
  <% end %>
</ul>
```

### put code into Model
```ruby
# controller
class UsersController < ApplicationController
  def index
    @user = User.ordered
  end
end 

# model
class User < ActiveRecord::Base
  def self.ordered
    order :last_name
  end
end
```

### scope

```ruby
# model
class User < ActiveRecord::Base
  scope :ordered, order(:last_name)
end
```
