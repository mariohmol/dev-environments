
# Active Record

Sometimes you might find errors saving models into the database, maybe because gems make extra steps that breaks the code.

To get errors messages, do:

```rb
begin 
    @mymodel.save!
rescue => error
    puts error
    puts error.backtrace
end

begin
  @mymodel.save! 
rescue ActiveRecord::RecordInvalid => invalid
  puts invalid.record.errors.messages
end
```


# Migration

## 4.2

Routes does not accept `to:` keyword, need to change to `action:`


## 5.0

Change all  Mailer that uses `deliver` to `deliver_now`


We have to go through every model , remove attr_accessible and use 
* https://edgeapi.rubyonrails.org/classes/ActionController/StrongParameters.html
* https://stackoverflow.com/questions/17371334/how-is-attr-accessible-used-in-rails-4

Looping over Request params has been changed and we were forced to make change like this:

```diff
-      params[:projeto][:metas_attributes].each do |goal|
+      params[:projeto][:metas_attributes].each_with_index do | goal, index|
```

strong params??

## 5.2

* Passing string to be evaluated in :if and :unless conditional options is not supported. Pass a symbol for an instance method, or a lambda, proc or block, instead.
** Solution:

```ruby
if: lambda { subscription.blank? }
# or the more common short-hand
if: ->{ subscription.blank? }
```

* undefined method `each_value' for #<ActionController::Parameters:0x00007fecb18e4270>
*SOLUTION* each_with_index and each_* does not exists for params anymore, use .
```ruby
params.each do |index, value| 
```

* There is no distinct method for Array, just for Active Records
```ruby
#before
params[:projetos].distinct  

# after
params[:projetos].uniq
```


* delete_all does not receive any paam anymore
```ruby
#before
entity.delete_all(id: id)

# after
entity.where(id: id).delete_all
```

* Sprockets::Rails::Helper::AssetNotPrecompiled in <%= stylesheet_link_tag "styles" %>
```ruby
# add to your app/config/environments/integration.rb
config.assets.check_precompiled_asset = false
```