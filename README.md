# Remote Distribution Load Testing in JMeter

Following are the troubleshooting steps to implement remote distribution load testing in JMeter.

* Same version of JMeter in master and worker
* Same version of Java in master and worker
* Master should be able to ping worker, vice-versa
* Master and Worker machines must be on the same sub-network. Here is the [calculator to check](https://www.meridianoutpost.com/resources/etools/network/two-ips-on-same-network.php).
* Firewall should be open
* Anti-virus might need to be turned off
* Delete the unnessary network interfaces in Windows to avoid the network related issues
* By default, JMeter uses SSL in RMI.
* SSL for RMI certificate has the validity of 7 days by default, make sure you generate the certificate once in 7 days. The number of days can be changed in the `create-rmi-keystore` file: `keytool -genkey -keyalg RSA -alias rmi -keystore rmi_keystore.jks -storepass changeit -validity 7 -keysize 2048 %*`
* Place the generated certificate in all the worker nodes
* If the test plan uses, plugins, make sure it is available in the worker nodes.
* Troubleshoot the remote testing by disable SSL in `jmeter.properties`

## Windows Troubleshooting

Make sure you enabled only below connections in your network.

![image](https://user-images.githubusercontent.com/2826376/116022784-d9e4ab80-a618-11eb-8654-422820dd2afd.png)

![image](https://user-images.githubusercontent.com/2826376/116022819-ec5ee500-a618-11eb-9bcd-b63d4ee0c526.png)
