###Mac 卸载mysql

先停止所有mysql有关进程

```
sudorm /usr/local/mysql
sudorm -rf /usr/local/mysql*
sudorm -rf /Library/StartupItems/MySQLCOM
sudorm -rf /Library/PreferencePanes/My*
sudovi /etc/hostconfig #removed the line MYSQLCOM=-YES-
rm -rf/Library/PreferencePanes/My*
sudo rm -rf/Library/Receipts/mysql*
sudo rm -rf/Library/Receipts/MySQL*
sudo rm -rf /var/db/receipts/com.mysql.*
```