#### docker 快速启动一个mysql
```
docker run -d -p 3306:3306 --name mysql-container -e MYSQL_ROOT_PASSWORD=myrootpassword -e MYSQL_DATABASE=mydb mysql:8.0
```
