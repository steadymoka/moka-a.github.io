---
layout: post
title: "rails api server"
modified: 2015-12-20T23:39:55-04:00
categories: blog
excerpt:
tags: [ruby, rails]
image:
  feature:
date: 2015-12-20T23:39:55-04:00
---

##### ruby korea advent-calendar 20일자 참여 글입니다.
 [https://ruby-korea.github.io/advent-calendar/](https://ruby-korea.github.io/advent-calendar/)
 
# Rails API server

{% highlight ruby %}
resources :episodes # http://localhost/episodes
resources :zombies, constraints: { subdomain: 'api' } # http://api.localhost/episodes
resources :humans, constraints: { subdomain: 'api' } # http://api.localhost/episodes
{% endhighlight %}

{% highlight ruby %}
resources :episodes # http://localhost/episodes

constraints subdomain: 'api' do
  resources :zombies # http://api.localhost/episodes
  resources :humans # http://api.localhost/episodes
end
{% endhighlight %}
서브도메인을 이용할수 있다.

하지만 이렇게만 하면, web site 코드와 api 코드가 같은 폴더아래에 놓이게 된다.
따라서

{% highlight ruby %}
constraints subdomain: 'api' do
  namespace :api do
    resources :zombies
  end
end
{% endhighlight %}
해주면 api/zombies#index {:subdomain=>"api"} 로 routing 된다.

{% highlight ruby %}
# app/controllers/api/zombies_controller.ruby
module Api
  class ZombiesController < ApplicationController
  end
end
{% endhighlight %}
여기서 또, URI 패턴을 보면, http://api.localhost/api/zombies 형태로 api 가 두번나와 중복이 된다.
이것도 없애 줄수 있다. 

{% highlight ruby %}
constraints subdomain: 'api' do
  namespace :api, path: '/' do
    resources :zombies
  end
end
{% endhighlight %}
해주면 http://api.localhost/zombies 로 중복도니 api 를 하나 없애줄수 있다.

<br>
{% highlight ruby %}
# config/initializers/inflections.rb
ActiveSupport::Inflector.inflections(:en) do |inflect|
  inflect.acronym 'API'
end

# app/controllers/api/zombies_controller.ruby
module API
  class ZombiesController < ApplicationController
  end
end
{% endhighlight %}


###### 출처 : CodeSchool-Surviving APIs with Rails