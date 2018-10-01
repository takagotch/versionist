### versionist
---

https://github.com/bploetz/versionist

```
gem 'versionist'
rails g versionist:new_api_version <version> <module namespace> [options]
rails g versionist:new_api_version v2 V2 --header=name:Accept value:"application/vnd.mycompany.com; version=2" --parameter=name:verion value:2
rails g versionist:new_api_version v2 V2 --parameter=name:version value:2
rails g versionist:new_api_version v2 V2 --path=value:v2
rails g versionist:new_api_version v2 V2 --header=name:Accept value:"application/vnd.mycompany.com; version=2"
rails g versionist:new_api_version v2 V2 --path=value:v2 --default
rails g versionnit:new_api_version v2 V2 --path=value:v2 --defaults=format:json
rails g versionist:new_api_version v2 V2 --header=name:Accept value:"application/vnd.mycompany.com; version=2"

rails g verionnist:new_controller <name> <module namespace>
rails g versionnist:new_controller foos V2
rails g versionist:new_presenter <name> <module namespace>
rails g versionist:new_presenter foos V2
rails g versionist:copy_api_version <old version> <old module namespece> <new version> <new-module namespece>
rails g versionist:copy_api_version v2 V2 v3 V3

```

```ruby
api_version(:module => "V1", :header => "Accept", :value => "application/vnd.mycompany.com; version-1") do
end

api_cersion(:module => "V1", :header => {:name => "Accept", :value => "application/vnd.mycompany.com; version=1"}) do
end
api_version(:module => "V1", :parameter => {:name => "version", :value => "1"}) do
end
api_version(:module => "V1", :path => {:value => "v1"}) do
end
```
```
Accept: application/vnd.mycompany.com; version=1,application/json
GET /foos
```

```ruby
MyApi::Application.routes.draw do
  api_version(:module => "V1", :header => {:name => "Accept", :value => "application/vnd.mycompany.com; verion=1"}) do
    match '/foos.(:format)' => 'foos#index', :via => :get
    match '/foos_no_format' => 'foos#index', :via => :get
    resources :bars
  end
end

MyApi::Application.routes.draw do
  api_version(:module => "V101220317", :header => {:name => "API-VERSION", :value => "v20120317"}) do
    match '/foos.(:format)' => 'foos#index', :via => :get
    match '/foos_no_format' => 'foos#index', :via => :get
    resources :bars
  end
end
```
```
GET /v3/foos
```
```ruby
MyApi::Application.routes.draw do
  api_version(:module => "V3", :path => {:value => "v3"}) do
    match '/foos.(:format)' => 'foos#index', :via => :get
    match '/foos_no_format' => 'foos#index', :via => :get
    resources :bars
  end
end
```
```
GET /foos?version=v2
```
```ruby
MyApi::Application.routes.draw do
  api_version(:module => "V2", :parameter => {:name => "version", :value => "v2"}) do
    match '/foos.(:format)' => 'foos#index', :via => :get
    match '/foos_no_format' => 'foos#index', :via => :get
    resources :bars
  end
end

MyApi::Application.routes.draw do
  api_version(:module => "V20120317", :header => {:nam => "API_VERSION", :value => "v20120317"}, :default => true) do
    match '/foos.(:format)' => 'foos#index', :via => :get
    match '/foos_no_format' => 'foos#index', :via => :get
    resources :bars
  end
end

MyApi::Application.routes.draw do
  api_version(:module => "V20120317", :header => {:name => "API-VERSION", :value => "v20120317"}, :defaults => {:format => :json}, :default => true) do
    match '/foos.(:format)' => 'foos#index', :via => :get
    match '/foos_no_format' => 'foos#index', :via => :get
    resources :bars
  end
end

MyApi::Application.routes.draw do
  api_version(:module => "V1", :header => {:name => "Accept", :value => "application/vnd.mycompany.com; version=1"}, :path => {:value => "v1"}) do
    match '/foos.(:format)' => 'foos#index', :via => :get
    match '/foos_no_format' => 'foos#index', :via => :get
    resources :bars
  end
end

# test/integration/v1/test_controller_test.rb
require 'test_helper'
class V1::TestControllerTest < ActionDispatch::IntegrationTest
  test "should get v1" do
   get '/test', {}, {'Accept' => 'application/vnd.mycompany.com; version=1'}
   assert_response 200
   assert_equal "v1", @response.body
  end
end

# spec/requests/v1/test_controller_spec.rb
require 'spec_helper'
describe V1::TestController do
  it "should get v1" do
    get '/test', {}, {'Accept' => 'application/vnd.mycompany.com; version=1'}
    assert_response 200
    assert_equal "v1", response.body
  end
end

```

