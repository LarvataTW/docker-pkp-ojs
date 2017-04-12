# PKP-OJS

Public Knowledge Project - [Open Journal Systems](https://github.com/pkp/ojs)

Open Journal Systems (OJS) is a journal management and publishing system that has been developed by the Public Knowledge Project through its federally funded efforts to expand and improve access to research.

OJS 是一套以 PHP 開發的開放源碼學術期刊管理系統。
2016 年 8 月 31 日正式發佈 OJS 3 版本，僅支援 PHP 5.3.7 以上。 

# Run OJS 3.x

To Do:

 - 將 OJS 的設定檔 config.inc.php 抽出來，放在 3.x/config.TEMPLATE.inc.php
 - 將 3.x/config.TEMPLATE.inc.php 內關於資料庫的設定值，抽成系統環境變數

## Docker Compose
Create a *docker-compose.yml* file and run ```docker-compose up```.
```dockerfile
# docker-compose.yml
ojs:
  image: 3.x/.
  ports:
    - 80:80
  links:
    - mysql:db
mysql:
  image: mysql
  environment:
    - MYSQL_ROOT_PASSWORD=root
    - MYSQL_DATABASE=ojs
    - MYSQL_USER=ojs
    - MYSQL_PASSWORD=ojs
```

# Run OJS 2.x

## Start containers

Go to the directory `2.x` and run `docker-compose up`. This will start three containers (for web application, database, and a data container - the latter one will immediately exit again).

You can find out the IP of the web container by running the following command: `docker network inspect 2x_default`.

To find out the name of the network of the just started containers, use `docker network ls`.

Then you can open the installation page at http://<ip of web container>/ojs/index.php.

To change the installation path of OJS to something other than `/ojs`, please see build arguments in `docker-compose.yml`.

使用 2.x 目錄內的 docker-compose.yml 啟動，會帶起來三個 container：

1. Web container (OJS 程式主體)
2. Database container (資料庫)
3. Data container (提供共享 Volume 的 Container)

## Configuration

Open the installation page at http://[ip of web container]/[path to ojs]/index.php. Make at least the following changes:

* Add admin user credentials
* Change the file upload directory to `/var/ojs/files`
* Set the database host to `db`
* Remove the tick in the checkbox "Create new database". The database was already created in the database container via the `docker-compose.yml`

## Start containers with own configuration

You can run the main containers and additional containers or overwrite settings with the following command:

```
docker-compose -f docker-compose.yml -f docker-compose.myextensions.yml {build, up, stop, start, down, rm}
```

*Be sure to list all these files again when stopping, downing, or removing the containers!*
