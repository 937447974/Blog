Ruby 访问服务器时，也可以通过 get 或 post 请求。

# 1 Get 请求

```ruby
require 'net/https'
require 'uri'
require 'json'

params = {}
params["username"] = 'yangjun'

# 发送
base_path = 'http://www.yjcocoa.com/prod_feige_service'
uri = URI.parse("#{base_path}/feige")
res = Net::HTTP.post_form(uri, params)

resbody = JSON.parse(res.body)
puts resbody
```

# 2 Post 请求

Ruby post 请求我们主要通过 json 交互。

```ruby
require 'net/https'
require 'uri'
require 'json'

params = {}
params["username"] = 'yangjun'

uri = URI.parse("#{base_path}/feige")
req = Net::HTTP::Post.new(uri.path, {'Content-Type' => 'application/json'})
req.body = params.to_json

res = Net::HTTP.new(uri.host, uri.port).start{|http|
    http.request(req)
}

resbody = JSON.parse(res.body)
puts resbody
```

&#160;

----------

# Appendix

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2017-11-9 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974