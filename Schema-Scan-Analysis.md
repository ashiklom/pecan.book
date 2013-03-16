Schema Scan is a tool commonly used to examine databases and view their structure and tables. 

<strong>How to use Schema Scan?</strong><br>
<p>
Step 1: Download the required software. <br>
* Download schema spy from <a href="http://sourceforge.net/projects/schemaspy/files/">http://sourceforge.net/projects/schemaspy/files/</a> be sure to remember the location you store the file. <br>
* Download a connector between mysql to java. <a href="http://dev.mysql.com/downloads/connector/j/5.0.html"> http://dev.mysql.com/downloads/connector/j/5.0.html</a> Once again remember the location.<br>
* Download graphviz. <a href="http://www.graphviz.org"> http://www.graphviz.org </a>
* You may also have to download and configure mysql to connect to your bety or the other database you are analysing.</p>
<p>Step 2: Open a command line and run the following sequence of code.<br>
java -jar [path to schema spy jar] -t -mysql -o [output folder] -host [name of host] -db [name of database] -u [username] -p [password] -dp [path to connector jar].<br>
Ex: 
java -jar /Users//Downloads/schemaSpy_5.0.0.jar  -t mysql -o bety -host localhost -db bety -u bety -p bety -dp /Users//Downloads/mysql-connector-java-5.0.8\ 2/mysql-connector-java-5.0.8-bin.jar <br></p>
Step 3: Your analysis you should now be properly created. To view the analysis open the index.html page with a browser. 
