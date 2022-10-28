# zeppelin-setup

1) Download Apache Zeppelin
   https://zeppelin.apache.org/download.html

   a)  Generate maprticket running
       $ maprlogin password  (Enter password)
       $ cp /tmp/maprticket.<uid>  to $ZEPPELIN_HOME/conf

   b) Edit $ZEPPELIN_HOME/conf/zeppelin-env.sh, add MAPR_TICKETFILE_LOCATION and JAVA_HOME
   #

   #MapR ticket service
   export MAPR_TICKETFILE_LOCATION=/opt/zeppelin/conf/maprticket
   #
   export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64

   c) Edit zeppelin-site.xml, enter IP address or DNS resolvable hostname 
  <property>
   <name>zeppelin.server.addr</name>
   <value>192.168.208.228</value>
   <description>Server binding address</description>
  </property>


2) Download Drill JDBC driver from below URL to /tmp location.
   http://package.mapr.com/tools/MapR-JDBC/MapR_Drill/MapRDrill_jdbc_v1.6.11.1008/MapRDrillJDBC-1.6.11.1008.zip
   More versions of the JDBC driver available on this page:
   - https://docs.datafabric.hpe.com/70/Drill/drill_jdbc_connector.html 
   
   $ cd /tmp
   $ unzip MapRDrillJDBC-1.6.11.1008.zip
   Run unzip again MapRDrillJDBC42-1.6.11.1008.zip 
   $ unzip MapRDrillJDBC42-1.6.11.1008.zip
   Copy all the jars to $ZEPPELIN_HOME/interpreter/jdbc/

3) Restart zeppelin
   $ $ZEPPELIN_HOME/bin/zeppelin-daemon.sh restart

4) Create new Interpreter, select "jdbc" as the category, name as "drill"
   Enter few fields:
   default.url: jdbc:drill:drillbit=<hostname>:31010;auth=maprsasl

    (For example: jdbc:drill:drillbit=rrl-ecp-demo-externaldf-host-1:31010;auth=MAPRSASL)

   default.user: mapr (for example)
   default.password: mapr (for example)
   default.driver: com.mapr.drill.jdbc.Driver
   Enable zeppelin.jdbc.auth.kerberos.proxy.enable: enable
 
4) Load the URL with the IP/hostame specified in zeppelin-site.xml
   https://192.168.208.228:8080

5) If there are Log errors, delete the jars from $ZEPPELIN_HOME/interpreter/jdbc/log4j-over-slf4j-1.7.25.jar 
   $ rm $ZEPPELIN_HOME/interpreter/jdbc/log4j-over-slf4j-1.7.25.jar 

6) In the zeppelin interface, create new note. Access drill 
   %drill
   %drill show databases
   
   
