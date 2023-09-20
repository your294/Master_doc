要将MySQL数据库表导出为文件并在其他电脑上导入，你可以使用MySQL提供的工具和命令行操作。以下是一般的步骤：

在源电脑上导出数据库表：

打开命令提示符或终端窗口。

使用mysqldump命令来导出数据库表。以下是导出单个表的示例命令：

```sql
mysqldump -u username -p database_name table_name > table_name_dump.sql
```
username：你的MySQL用户名。
database_name：要导出表所在的数据库名称。
table_name：要导出的表名称。
table_name_dump.sql：导出的数据将保存在这个文件中。
在执行此命令后，系统将提示你输入MySQL密码。

输入MySQL密码并按回车键。如果密码正确，mysqldump将导出表数据到指定的文件。

在目标电脑上导入数据库表：

将在源电脑上导出的SQL文件（例如table_name_dump.sql）传输到目标电脑上。你可以使用文件传输工具，如USB驱动器、网络共享或云存储来完成这个步骤。

在目标电脑上，打开命令提示符或终端窗口。

使用mysql命令来导入数据库表。以下是导入的示例命令：

```sql
mysql -u username -p database_name < table_name_dump.sql
```
username：你的MySQL用户名。
database_name：要导入表的目标数据库名称。
table_name_dump.sql：包含表数据的SQL文件。
在执行此命令后，系统将提示你输入MySQL密码。

输入MySQL密码并按回车键。如果密码正确，mysql将读取SQL文件并将表数据导入到目标数据库中。

此时，你的数据库表已成功从源电脑导出并导入到目标电脑。请确保目标电脑上已经安装了MySQL，并且你具有足够的权限来执行导入操作。如果在导入过程中遇到任何问题，请确保检查SQL文件的完整性和格式。