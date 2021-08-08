To inform graphics code that there is no graphics console available, and to set tomcat with maximum 1024Mb memory in use. 

```
 JAVA_OPTS="-server -Xms256m -Xmx1024m -Djava.awt.headless=true"
 export JAVA_OPT
```

Tomcat script, please note the chkconfig line specifying “5 86 16” tells the operating system to start and run the script at runlevel 5, followed by the order to start and stop the script in relation to the other scripts in run level 5 (rc.5). Perform the this command when it is created /sbin/chkconfig –add tomcat

```
 #!/bin/bash
 #
 # tomcat
 #
 # chkconfig: 5 86 16
 # description: Start up the Tomcat servlet engine.
 #
 # Source function library.
 . /etc/rc.d/init.d/functions
 if [ -f /etc/sysconfig/tomcat ]; then
 . /etc/sysconfig/tomcat
 fi
 prog=tomcat
 user=tomcat
 pidfile=${PIDFILE-/var/run/tomcat.pid}
 lockfile=${LOCKFILE-/var/lock/subsys/tomcat}
 pid=0
 export JAVA_HOME=/opt/jdk_default
 RETVAL=0
 start() {
 echo -n $"Starting $prog: "
 /bin/su $user /opt/tomcat/bin/startup.sh
 RETVAL=$?
 echo
 [ $RETVAL = 0 ] && touch ${lockfile}
 return $RETVAL
 }
 stop() {
 echo -n $"Stopping $prog: "
 /bin/su $user /opt/tomcat/bin/shutdown.sh
 RETVAL=$?
 echo
 [ $RETVAL = 0 ] && rm -f ${lockfile} ${pidfile}
 }
 # See how we were called.
 case "$1" in
 start)
 start
 ;;
 stop)
 stop
 ;;
 status)
 pid=`/bin/ps --no-heading -U $user -o pid`
 if [ -n "$pid" ]; then
 echo $"Tomcat (pid $pid) is running..."
 else
 echo "Tomcat is stopped"
 fi
 RETVAL=$?
 ;;
 restart)
 stop
 start
 ;;
 condrestart)
 if [ -f ${pidfile} ] ; then
 stop
 start
 fi
 ;;
 reload)
 stop
 start
 ;;
 *)
 echo $"Usage: $prog {start|stop|restart|condrestart|reload|status}"
 exit 1
 esac
 exit $RETVAL
```

Enable Tomcat SSL 

```
 # Generate keystore and cert with following command:
 /usr/java/default/bin/keytool -genkey -alias <server_ipaddress> -keyalg RSA -validity 20999 -keystore my.keystore
 #uncomment below lines at <tomcat_dir>/conf/server.xml
 <Connector port="8443" protocol="HTTP/1.1" SSLEnabled="true" maxThreads="150" scheme="https" secure="true" keystoreFile="/home/choojun/apache-tomcat-6.0.26/webapps/my.keystore" keystorePass="my_password" clientAuth="false" sslProtocol="TLS" />
 # Use below java application to test SSL remote accessing:
 import javax.net.ssl.SSLSocket;
 import javax.net.ssl.SSLSocketFactory;
 import java.io.*;
 /**
  * Establish a SSL connection to a host and port, writes a byte and * prints the
  * response.
  * */
 public class SSLPoke
 {
    public static void main(String[] args)
    {
        if (false && args.length != 2)
        {
            System.out.println("Usage: " + SSLPoke.class.getName() + " <host> <port>");
            System.exit(1);
        }
        // String ip = args[0];
        String ip = "192.168.1.100";
        // String port = args[1];
        String port = "8443";
        // require VM argument below
        // -Djavax.net.ssl.trustStore=/home/choojun/Documents/local_copy/my.keystore
        // -Djavax.net.ssl.trustStorePassword=my_password
        try
        {
            SSLSocketFactory sslsocketfactory = (SSLSocketFactory) SSLSocketFactory
                    .getDefault();
            SSLSocket sslsocket = (SSLSocket) sslsocketfactory.createSocket(ip,
                    Integer.parseInt(port));
            InputStream in = sslsocket.getInputStream();
            OutputStream out = sslsocket.getOutputStream();
            // Write a test byte to get a reaction :)
            out.write(1000);
            while (in.available() > 0)
            {
                System.out.print(in.read());
            }
            System.out.println("Successfully connected");
        }
        catch (Exception exception)
        {
            exception.printStackTrace();
        }
    }
 }
```
