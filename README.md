# Laradock
官網: http://laradock.io/getting-started/

```
git clone https://github.com/Laradock/laradock.git

mkdir app
```

複製 .env
```
cd laradock

cp .env.example .env
```

修改 .env 路徑或port
```
...

# Point to the path of your applications code on your host
APP_CODE_PATH_HOST=../app

# Point to where the `APP_CODE_PATH_HOST` should be in the container
APP_CODE_PATH_CONTAINER=/var/www

...
```
現在看起來資料結構應該長這樣  
<img width="639" alt="structure_env" src="https://user-images.githubusercontent.com/24542187/150494467-3f18fa27-b586-408f-ac5e-e658fff73bcc.png">


啟動container
```
docker-compose up -d nginx phpmyadmin
# 因為相依的關係，其實會啟動 php-fpm，mysql等 container
```

若遇到
ERROR [internal] load metadata  
可參閱 https://learnku.com/laravel/t/60298  
注意 **platform** 可能是關鍵字


# 建立專案
進入workspace  
```
docker-compose exec workspace bash
```
預設進入的資料夾是`/var/www/`  
可在`.env`的`APP_CODE_PATH_CONTAINER`修改  
注意這裡是binding的對應關係(volume)，local端對應workspace的關係  
如果設定/var/www/myproject，則workspace會在/var/www下自動生成myproject資料夾

因為上面綁定的是 `/app` 底下對應 `/var/www`
app底下是空的，所以現在看到的www底下應也是空的

ps:安裝好後若進入nginx容器，會發現預設的網站路徑是`/var/www/public`  
若直接在預設的/`var/www`下create，nginx會出現404找不到路徑  
應該有地方可以設定，但目前直接cd到上層

```
cd ..
composer create-project --prefer-dist laravel/laravel www
```

下了這指令之後，資料就綁定到 app 下了  
並且因為有對應到 nginx 預設的路徑 `/var/www/public`  
這時瀏覽 http://localhost/ 應該就能看到laravel預設畫面

