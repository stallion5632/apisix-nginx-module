### this repository is a part of https://github.com/api7/apisix-nginx-module

### To realize  the https://github.com/openresty/meta-lua-nginx-module/pull/76

### how  to use
+ base openresty official 1.19.9.1 version
+ Follow the steps below:
  + cd apisix-nginx-module/patch 
  + ./patch.sh ThePathOfYourOpenRestySrcDirectory     
  + cd openresty-1.19.9.1 && ./configure  --add-module=../apisix-nginx-module/src/meta
  + make -j10
  +  
### 测试nginx.conf
```
lua {
    lua_shared_dict dogs 1m;
}
http {

    server {
        listen 9991;
        location /t {
            content_by_lua_block {
                ngx.shared.dogs:set("foo", "abcd")
                print(ngx.shared.dogs:get("foo")) -- works
             }
        }

    }
}
stream {
    server {
        listen 9992;
        content_by_lua_block {
            print(ngx.shared.dogs:get("foo")) -- works too!
        }
    }
}
```
