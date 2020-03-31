# Install unixODBC

> unixODBC-2.3.7

http://www.unixodbc.org/<br>

+ ./configure --prefix=/usr/local/unixODBC --sysconfdir=/usr/local/unixODBC/etc
+ make
+ sudo make install

+ go to ~/Qt5.14.1/5.14.1/Src/qtbase/src/plugins/sqldrivers/odbc
+ edit odbc.pro

> \#QMAKE_USE += odbc

+ sudo ~/Qt5.14.1/5.14.1/gcc_64/bin/qmake "INCLUDEPATH+=/usr/local/unixODBC/include" "LIBS+=-L/usr/local/unixODBC/lib -lodbc"
+ sudo make
+ sudo make install

> output file : libqsqlodbc.so  libqsqlodbc.so.debug

> output path : ~/Qt5.14.1/5.14.1/Src/qtbase/src/plugins/sqldrivers/plugins/sqldrivers

+ cp libqsqlodbc.so to ~/Qt5.14.1/5.14.1/gcc_64/plugins/sqldrivers/

# Install freetds

> freetds-1.1.24 or 1.1.26

https://www.freetds.org/<br>

+ ./configure --enable-msdblib --prefix=/usr/local/freetds --with-tdsver=7.1 --disable-libiconv --with-unixodbc=/usr/local/unixODBC
+ make
+ sudo make install

# Edit odbcinst.ini

+ sudo vim /usr/local/unixODBC/etc/odbcinst.ini

```ini
[FreeTDS]
Description=FreeTDS Driver
Driver=/usr/local/freetds/lib/libtdsodbc.so
```

+ sudo cp /usr/local/unixODBC/etc/odbcinst.ini /etc/

# sample

```cpp
    QString dsn, qsDataBaseError, qsDriverError, qsIP;
    char account[100], pwd[100], database[100];
    db = QSqlDatabase::addDatabase("QODBC");
    
    qsIP = "xxx.xxx.xxx.xxx";
    strcpy(database, "xxxx");
    strcpy(account, "xxxx");
    strcpy(pwd, "xxxx");
    dsn = QString::asprintf("DRIVER={FreeTDS};SERVER=%s,1433;DATABASE=%s;UID=%s;PWD=%s;", 
                             qsIP.toUtf8().data(), 
                             database, 
                             account, 
                             pwd);
    db.setDatabaseName(dsn);
    if(db.open())
    {
        return true;
    }
    
    qsDataBaseError = db.lastError().databaseText();
    qsDriverError = db.lastError().driverText();
    return false;
    
```
