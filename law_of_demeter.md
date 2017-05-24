### problems 
```ruby
class Address < ActiveRecord::Base
  belongs_to :customer
end

class Customer <ActiveRecord::Base
  has_one :address
  has_many :invoices
end

class Invoice < ActiveRecord::Base
  belongs_to :customer
end
```

```html
<%= @invoice.customer.name %>
<%= @invoice.customer.address.city %>
<%= @invoice.customer.address.street %>
<%= @invoice.customer.address.state %>
```
### solutions
1. use only one dot 
```ruby
class Address < ActiveRecord::Base
  belongs_to :customer
end

class Customer < ActiveRecord::Base
  has_one :address
  has_many :invoices
  
  def street
    address.street
  end
  
  def city
    address.city
  end
end

class Invoice < ActiveRecord::Base
  belongs_to :customer
  
  def customer_name
    customer.name
  end
  
  def customer_street
    customer.street
  end
  
  def customer_city
    customer.city
  end
end
```

```html.erb
  <%= @invoice.customer_name %>
  <%= @invoice.customer_city %>
  <%= @invoice.customer_street %>
```

> tooo many methods
2. Use delegate
```ruby
class Address
  belongs_to :customer
end

class Customer
  has_one :address
  has_many: invoices
  
  delegate :street, :city, :zip_code, to: :address
end

class Invoice
  belongs_to :customer
  
  delegate :name, :street, :city, :zip_code, to: :address, prefix: true
end
```

```erb
<%= @invoice.customer_name %>
<%= @invoice.customer_street %>
<%= @invoice.customer_address %>
...
```
