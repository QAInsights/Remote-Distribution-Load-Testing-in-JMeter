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

## Commands

### Starting Worker Machine

`./jmeter-server -Djava.rmi.server.hostname=Worker_IP`

**Sample Output**

```
Using local port: 4000
Created remote object: UnicastServerRef2 [liveRef: [endpoint:[192.168.0.99:4000](local),objID:[59f63471:179a76f79c7:-7fff, -5372372549273377610]]]
```

### Starting Tests Master Machine 

`cd into JMeter folder`

`./jmeter.sh -Djava.rmi.server.hostname=192.168.0.98 -n -t examples/CSVSample.jmx -l Run1.log -R192.168.0.99`

**Sample Output in Master**

```
Creating summariser <summary>
Created the tree successfully using examples/CSVSample.jmx
Configuring remote engine: 192.168.0.99
Using local port: 4000
Starting distributed test with remote engines: [192.168.0.99] @ Wed May 26 02:50:45 EDT 2021 (1622011845669)
Remote engines have been started:[192.168.0.99]
Waiting for possible Shutdown/StopTestNow/HeapDump/ThreadDump message on port 4445
summary =     12 in 00:00:04 =    3.3/s Avg:   234 Min:   123 Max:   326 Err:     0 (0.00%)
Tidying up remote @ Wed May 26 02:50:51 EDT 2021 (1622011851137)
```

**Sample Output in Worker**

```
Starting the test on host 192.168.0.99 @ Wed May 26 02:50:47 EDT 2021 (1622011847465)
Finished the test on host 192.168.0.99 @ Wed May 26 02:50:51 EDT 2021 (1622011851480)
```

## Disabling Firewall in CentOS

To check the status:

`sudo firewall-cmd --state`

To stop the firewall:

`sudo systemctl stop firewalld`
