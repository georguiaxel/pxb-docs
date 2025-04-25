# Restore full, incremental, and compressed backups

> **Warning:**  
> Backup needs to be prepared before it can be restored.

The restore backup procedure is the same for full, incremental, and compressed backups.

For convenience, *xtrabackup* binary has the `--copy-back` option to copy the backup to the datadir of the server:

```{.bash data-prompt="$"}
$ xtrabackup --copy-back --target-dir=/data/backups/
```

If you don’t want to save your backup, you can use the `--move-back` option which will move the backed up data to the datadir.

If you don’t want to use any of the above options, you can additionally use
rsync or cp to restore the files.

!!! note
   
    The datadir must be empty before restoring the backup. Also, it’s important to note that MySQL server needs to be shut down before restore is performed. You cannot restore to a datadir of a running mysqld instance (except when importing a partial backup).

# Example of the rsync command that can be used to restore the backup
can look like this:

```{.bash data-prompt="$"}
$ rsync -avrP /data/backup/ /var/lib/mysql/
```

You should check that the restored files have the correct ownership and permissions.

As files’ attributes are preserved, in most cases you must change the files’ ownership to `mysql` before starting the database server, as the files are owned by the user who created the backup:

```{.bash data-prompt="$"}
$ chown -R mysql:mysql /var/lib/mysql
```
Data is now restored, and you can start the server.

# Additional Steps for Full Restoration:

If needed, you can perform additional steps for restoring the database from a backup. For example:

```{.bash data-prompt="$"}

$ sudo mv /var/lib/mysql /var/lib/mysql.bak.$(date +%s)

$ sudo cp -r /home/ubuntu/mysql_backups/full /var/lib/mysql

$ sudo chown -R mysql:mysql /var/lib/mysql
```
The commands above move the existing MySQL data directory to a backup folder with a timestamp and then copy the full backup data to the MySQL data directory.

# Start the MySQL Server: Once the restoration is complete and the ownership is corrected, you can start the MySQL server.

```{.bash data-prompt="$"}
$ sudo systemctl start mysql
```
Your MySQL server should now be restored from the backup and ready to use.

