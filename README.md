Auto-logout mechanism for Megaplan system.
Автоматически разлогинивает пользователей из системы Мегаплан, когда количество активных лицензий подходит к концу.

Put ```miniplan``` wherever, run it by cron.

Put this code in ```/etc/nginx/megaplan/userlive.redis.conf```:
```
location = /userLive {
    rewrite_by_lua '
    local user_id = ngx.var[\'arg_uid\']
    local domain_name = ngx.var[\'http_host\']
    local redirect_url = "https://"..domain_name.."/logout"
    local file = io.open("/tmp/uids.txt", "r")
    local line = file:read()
        --ngx.log(ngx.ERR, "==== Redirecting to: "..redirect_url.." ====")
        if line == user_id then
            ngx.redirect(redirect_url)
            ngx.exit(503)
            end
        ';
```
