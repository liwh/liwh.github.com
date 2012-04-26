---
layout: post
title: "build-oauth2-provider"
date: 2011-11-05 20:10
comments: true
categories:
---

### introduce

最近，花了一段时间去整理oauth2.0 provider的功能，现在，很多互联网都是通过此方式共享API接口，
例如，豆瓣，新浪等等。其特点是不需要用户在第三方应用中输入用户名、密码即可完成授权操作。现在最新版本是 [draft-ietf-oauth-v2-22](http://tools.ietf.org/html/draft-ietf-oauth-v2-22),具体流程可参考其文档。这里就不赘述其细节了，简单来说就是通过client_id和client_secret还有redirect_uri这几个参数，请求验证服务器的授权，然后通过返回一个可信任的access_token来调用服务器的资源。下面，介绍一下如何在rails 中实现这个流程。


### 实现

在github上查找了几个gem,  最终选定 [oauth2-provider](https://github.com/songkick/oauth2-provider) ，下面介绍一下实现方式。


首先，我们创建一个oauth_controller,添加下面代码

```ruby oauth_controller.rb
class OauthController < ApplicationController
  def authorize # 返回authorize_code
    @oauth2 = OAuth2::Provider.parse(current_user.shop, request)
    #response.headers = @oauth2.response_headers
    #response.status = @oauth2.response_status
    redirect_to @oauth2.redirect_uri  and return if @oauth2.redirect?
  end

  def access_token # 返回access_token，记得返回值需为json
    @oauth2 = OAuth2::Provider.parse(nil, request)
    render json: @oauth2.response_body
  end

  def allow  #用户授权
    @auth = OAuth2::Provider::Authorization.new(current_user, params)
    if params['allow'] == '1'
      @auth.grant_access!
    else
      @auth.deny_access!
    end
    redirect_to @auth.redirect_uri
  end
end

```

在config/routes.rb中添加下面路由

```ruby config/routes.rb
     get '/oauth/authorize'    , to: 'oauth#authorize'   , as: :authorize
     post '/oauth/access_token', to: 'oauth#access_token', as: :access_token
     match '/oauth/allow'      , to: 'oauth#allow'       , as: :oauth_allow
```

现在来说，基本流程就定义清楚了。

这时用户已经能通过如下方式请求授权了

```ruby client_demo.rb
require 'oauth2'
client = OAuth2::Client.new('client_id', 'client_secret', :site => 'https://example.org')

client.auth_code.authorize_url(:redirect_uri => 'http://localhost:8080/oauth2/callback')
# => "https://example.org/oauth/authorization?response_type=code&client_id=client_id&redirect_uri=http://localhost:8080/oauth2/callback"

token = client.auth_code.get_token('authorization_code_value', :redirect_uri => 'http://localhost:8080/oauth2/callback')
response = token.get('/api/resource', :params => { 'query_foo' => 'bar' })
response.class.name
# => OAuth2::Response

```

### 过滤器

当然，这里在我们服务器上得加个过滤器，去验证用户是否是正确的。

```ruby application_controller.rb
  def login_or_oauth_required
    unless session[:shop]
      token = OAuth2::Provider.access_token(nil, [], request)
        unless token.valid?
          render json: {error: '[API] Invalid API key or permission token (unrecognized login or wrong password)'}
        else
          session[:shop] ||= token.owner.as_json(only: [:deadline, :created_at, :updated_at, :name])['shop']
        end
     end
  end
```

### 参考资料

[github-oauth2](http://developer.github.com/v3/oauth/)
[oauth2中文](http://wenku.baidu.com/view/b37ed7260722192e4536f66e.html)
[oauth2-spec](http://oauth.net/2/)



