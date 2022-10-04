# Tc3 OPC-UA Sever Startup Procedures



| Software Needed                     | Link                         |
| ----------------------------------- | ---------------------------- |
| TwinCAT 3 OPC UA Client             | https://tinyurl.com/bdzdwb94 |
| TwinCAT 3 OPC UA Server             | https://tinyurl.com/bdzdwb94 |
| TwinCAT 3 OPC UA Configurator       | https://tinyurl.com/bdzdwb94 |
| TwinCAT 3 OPC UA Gateway (optional) | https://tinyurl.com/bdzdwb94 |
| Unified Automation UaExpert         | https://tinyurl.com/46n3hsfb |



### Purpose

Provide first-time setup and testing of a TF6100 OPC-UA Server using Anonymous login with no certificate security. This allows you to test the validty of the OPC-UA connection before moving onto security setup. 

*This setup is only for **Windows 10 Target Devices**.*



### Step-by-Step Guide

Install TF6100 OPC-UA Server on the target device that is intended to host the sever. 

![image-20221004101812215](OPC-UA Sever Startup Procedures.assets/image-20221004101812215.png)

During installation select the none/none endpoint option. This allows you to connect to the server with no security certificates. 

![image-20221004101834597](OPC-UA Sever Startup Procedures.assets/image-20221004101834597.png)

On the development PC, install & open Unified Automation UaExpert. Also this is a good time to verify that either the firewall of both the Target Device and the Development PC have port 4840 open or have the firewall(s) turned off. 

Select the + icon on the toolbar to add a server to the UaExpert project. 

![image-20221004101909252](OPC-UA Sever Startup Procedures.assets/image-20221004101909252.png)

Under the Custom Discovery branch, double click to Add Server ; then enter the OPC-UA server address of your Target Device. In my instance its using the IP address of the port used and port number 4840. Click OK when finished.

![image-20221004101935755](OPC-UA Sever Startup Procedures.assets/image-20221004101935755.png)

If the OPC-UA server is reachable it will automatically popluate the Custom Discovery branch with the endpoint options for connecting. Select **None - None** & and leave Authentication Settings on **Anonymous**. Click OK when finished.

![image-20221004102323665](OPC-UA Sever Startup Procedures.assets/image-20221004102323665.png)

The Target Device server should now show up under the server branch of the UaExpert project. Right click on the server and select connect.

![image-20221004102357488](OPC-UA Sever Startup Procedures.assets/image-20221004102357488.png)

Since this is the first time connecting, it will ask you to valdiate the certifcate of the server. Select Trust Server Certificate and continue. 

![image-20221004102611557](OPC-UA Sever Startup Procedures.assets/image-20221004102611557.png)

You should now see the successfully validated certificate as "trusted". Click continue. 

![image-20221004102731588](OPC-UA Sever Startup Procedures.assets/image-20221004102731588.png)

Now that you are connected to the Target Device, you should have access to the Root folder in the Address Space. Under the Objects folder, find the **TrustOnFirstUse** method. Right click and select Call...

![image-20221004103015671](OPC-UA Sever Startup Procedures.assets/image-20221004103015671.png)

In the method call input arguments, enter a Username and Password. This will create a user that can be used in place of the Anonymous login. When finished filling out the arguments, select Call.

![image-20221004103355911](OPC-UA Sever Startup Procedures.assets/image-20221004103355911.png)

You should now see that the Result of the call is that it Succeded. Go ahead and close this window.

![image-20221004103505543](OPC-UA Sever Startup Procedures.assets/image-20221004103505543.png)

UaExpert will get booted from the Target Device as after the method call it will no longer accept Anonymous as the login. Go to properties of the server in the UaExpert project.

![image-20221004103627118](OPC-UA Sever Startup Procedures.assets/image-20221004103627118.png)

Under Authentication Settings, change from Anonymous to your newly created username and password. Check the store box and click OK.

![image-20221004103735688](OPC-UA Sever Startup Procedures.assets/image-20221004103735688.png)

Now connect to the Target Device by right clicking on the server and selecting Connect.

![image-20221004103819956](OPC-UA Sever Startup Procedures.assets/image-20221004103819956.png)

 If you are able to connect successfully, the **TrustOnFirstUse** method part of the procedure is now complete. We will move onto the TwinCAT OPC-UA Configurator to set up the server for accessing the variables in the TwinCAT project.

Open TwinCAT OPC-UA Configurator on the development PC. Click edit next to the server selection dropdown. Enter the Target Device Server URL and select Get Endpoints, choose **None-None** and Add.

![image-20221004111543436](OPC-UA Sever Startup Procedures.assets/image-20221004111543436.png)

![image-20221004111841726](OPC-UA Sever Startup Procedures.assets/image-20221004111841726.png)

Close the Configure Sever window, and then select Connect. In the Message log you should see that you have now successfully connected to the server.

![image-20221004112123659](OPC-UA Sever Startup Procedures.assets/image-20221004112123659.png)

Once connection is established and confirmed, select the Red Folder icon from the toolbar. This will open a server configuration file from the connected server. This pulls in the previously set up user names and any other configurations set up prior to this point for the Target Device server.

![image-20221004112310381](OPC-UA Sever Startup Procedures.assets/image-20221004112310381.png)

You will see a device added to the configurator and a message in the log that a configuration has been read successfully. 

## need to look at the AmsNetId and what the field means in this instance.....

![image-20221004112805812](OPC-UA Sever Startup Procedures.assets/image-20221004112805812.png)



Confirm the configuration is as expected by reviewing the Security tab. You should see the username you created earlier, in my instance under **Users** I see 'shawn".  Once confirmed, use the activate on target icon on the toolbar to complete the configuration on the target device.

![image-20221004113357720](OPC-UA Sever Startup Procedures.assets/image-20221004113357720.png)

![image-20221004113726811](OPC-UA Sever Startup Procedures.assets/image-20221004113726811.png)

![image-20221004113749486](OPC-UA Sever Startup Procedures.assets/image-20221004113749486.png)



## Setting up Tc3 Project to expose tags for the OPC-UA Server (as Filtered types).



Under the Tc3 Project Settings, make sure the Target Files "TMC File" checkbox is selected.

![image-20221004114219998](OPC-UA Sever Startup Procedures.assets/image-20221004114219998.png)



Declare variables with the OPC-UA attribute before each variable you would like exposed to the server. 

![image-20221004114316351](OPC-UA Sever Startup Procedures.assets/image-20221004114316351.png)

![image-20221004114422360](OPC-UA Sever Startup Procedures.assets/image-20221004114422360.png)

![image-20221004114433519](OPC-UA Sever Startup Procedures.assets/image-20221004114433519.png)



















