# Laradock
官網: http://laradock.io/getting-started/

```
git clone https://github.com/Laradock/laradock.git
```

```
cd laradock
docker-compose up -d nginx mysql
# 可以直接下nginx phpmyadmin，因為相依的關係，其實會啟動 php-fpm，mysql等 container
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

安裝好後若進入nginx容器，會發現預設的網站路徑是`/var/www/public`  
若直接在預設的/`var/www`下create，nginx會出現404找不到路徑  
應該有地方可以設定，但目前直接cd到上層
```
cd ..
composer create-project --prefer-dist laravel/laravel www
```

# 測試
create專案後，應該已經綁定到`.env`設定的APP_CODE_PATH_HOST  
直接修改專案`public/index.php`
```
<?php
echo phpinfo();
exit;
```

瀏覽 http://localhost/
