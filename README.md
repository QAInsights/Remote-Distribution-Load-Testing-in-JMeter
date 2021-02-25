# Remote Distribution Load Testing in JMeter

Following are the troubleshooting steps to implement remote distribution load testing in JMeter.

* Same version of JMeter in master and worker
* Same version of Java in master and worker
* Master should be able to ping worker, vice-versa
* Firewall should be open
* Anti-virus might be turned off
* Delete the unnessary network interfaces in Windows to avoid the network related issues
* By default, JMeter uses SSL in RMI.
* SSL for RMI certificate has the validity of 7 days by default, make sure you generate the certificate once in 7 days. The number of days can be changed in the `create-rmi-keystore` file: `keytool -genkey -keyalg RSA -alias rmi -keystore rmi_keystore.jks -storepass changeit -validity 7 -keysize 2048 %*`
* Place the generated certificate in all the worker nodes
* If the test plan uses, plugins, make sure it is available in the worker nodes.
* Troubleshoot the remote testing by disable SSL in `jmeter.properties`
