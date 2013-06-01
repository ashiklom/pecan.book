You may need to disable cookies specifically for the pecan webserver in your browser. This shouldn't be a problem running from the virtual machine, but your installation of php can include a 'PHPSESSID' that is quite long, and this can overflow the params field of the workflows table, depending on how long your hostname, model name, site name, etc are. 

If you are seeing: `Warning: mkdir() [function.mkdir]: No such file or directory in /path/to/pecan/web/runpecan.php at line 169` it is because you have used a relative path for $output_folder in system.php.

If you are seeing 
`Can't insert workflow : Incorrect integer value: '' for column 'advanced_edit' at row 1`, you are running mysql in strict mode. Open /usr/local/mysql/my.cnf or wherever you have installed mysql ('locate my.cnf' may help you) and comment out "sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES". Restart your mysql daemon using 'sudo /Library/StartupItems/MySQLCOM/MySQLCOM restart' and try again.
