# 10. Audit|审计

The systems provides a sophisticated audit log, that can be viewed in the WebUI.

系统提供了一个复杂的审计日志，可以在Web界面中查看。

![auditlog](../Contents/auditlog.png)

Audit Log(审计日志)

privacyIDEA comes with an SQL audit module. (see Audit log)

privacyIDEA自带一个SQL审计模块。（请参阅Audit log）

## 10.1. Cleaning up entries|清除条目

The sqlaudit module writes audit entries to an SQL database. For performance reasons the audit module does no log rotation during the logging process.

sqlaudit模块将审计条目写入SQL数据库。出于性能原因，审计模块在日志记录过程中不会进行日志更迭。

But you can set up a cron job to clean up old audit entries.

但是您可以设置一个cron作业来清除旧的审计条目。

You can specify a highwatermark and a lowwatermark. To clean up the audit log table, you can call pi-manage at command line:

您可以指定水平高低。要清除审计日志表，可以在命令行调用pi-manage：

```
pi-manage rotate_audit --highwatermark 20000 --lowwatermark 18000
```

This will, if there are more than 20.000 log entries, clean all old log entries, so that only 18000 log entries remain.

### 10.1.1. Access rights

You may also want to run the cron job with reduced rights. I.e. a user who has no read access to the original pi.cfg file, since this job does not need read access to the SECRET or PEPPER in the pi.cfg file.

So you can simply specify a config file with only the content:

```
PI_AUDIT_SQL_URI = <your database uri>
```

Then you can call pi-manage like this:

```
PRIVACYIDEA_CONFIGFILE=/home/cornelius/src/privacyidea/audit.cfg \
pi-manage rotate_audit
```

This will read the configuration (only the database uri) from the config file audit.cfg.

### 10.1.2. Table size

Sometimes the entires to be written to the database may be longer than the column in the database. You can either enlarge the columns in the database or you can set

```
PI_AUDIT_SQL_TRUNCATE = True
```

in pi.cfg. This will truncate each entry to the defined column length.