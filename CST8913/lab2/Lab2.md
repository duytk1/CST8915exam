# Describe how this application can be deployed in the cloud using IaaS infrastructure

Since this is the IaaS target architecture to deploy the application to the Azure cloud we must first create the infrastructure and configure them. This includes creating Virtual Machine (VM), Virtual Network (VNET), a Network Security Group (NSG) on Azure interface. The next step would be to deploy the application on Azure.

* ## **Creating Virtual Machine (VM)**

To create the VM we need to go on the Azure page, click on Virtual Machines, and click Create  
Then we configure the VM with Ubuntu 22.04 and region Canada central, and we choose the method of authentication with username and password.

* ## **Creating a Virtual Network (VNET)**

To create a VNET we go on the Azure front page, then click Virtual Networks, then click Create:  
Then we configure the VNET settings with Canada central and the IP addresses

* ## **Create a Network Security Group (NSG)**

To create NSG we need to go on the Azure front page, click on Network Security Groups and click Create:  
Then we configure it to be the region Canada Central

* ## **Deploying the app**

Then we would deploy the Flask app on the Back end, and deploy the React app on the front end by copying the code to the VM.

* ## **Run the app**

\-Since this is a Flask app it is likely to be in Python. To run the app we would need to install the dependency first. But before we can do that we need to install Python on the VM. To do that we need to go to the Python main page, download it and install it.  
\-There should be a requirement.txt file. To install the dependency on the Flask app we would run the command:  
pip install \-r requirements.txt  
After this we would run the app by running the file containing the main function. Let’s assume the file is named app.py, we would run it with the command:  
python app.py  
\-For the React app we need to download Node and install it. To do that we need to go to the Node front page, download and install it.  
\-And we can install the dependencies. To do that we run the commands:  
npm install  
\-Then we would run the React app by this command:  
npm run build

* ## **Test the app connection:**

After successfully deploying the app to Azure cloud we can test the app by opening a browser such as Microsoft Edge and go to the app’s IP address, which should be found in the configuration file of the Flask app.

# Target Architecture Diagram:

!(./architecture\_iaas.png)  
In this diagram we have the components:

* Azure: this is the service that Microsoft provides us.  
* VNET: This VNET will cover everything and all resources inside this VNET will be able to communicate with each other.  
* NSG: the network security group will contain all connection rules such as how limited or how the VM can connect with each other.  
* VM: this VM will contain the Flask App and the React App.  
* Flask App: this is the back end application and will perform the functionalities.  
* React App: this front end application will take the data from the Flask app and display it to the browsers inside the VNET that are accessing it.

# Describe how this application can be deployed in the cloud using PaaS infrastructure.

To deploy the app using PaaS target architecture, we would need to create App Services for both the Flask app and the React app separately, then we create a Virtual Network, a Network Security Group and configure them to the App Services. Then we would set up the routing between the Flask app and the React app.

* ## **Create App Services**

To do this we would go to the Azure front page, select App Services and click Create:  
And for the Flask app we would choose the stack as Python 3.12  
For the React app we would choose the Node 20 LTS for the stack

* ## **Then we create the VNET and NSG**

* ## **Routing**

We need to configure the routing between the Flask and the React app so they can see each other through the VNET.

# Target Architecture Diagram:

!(./architecture\_paas.png)  
Similarly to the IaaS architecture we have Azure and VNET to connect everything. But for PaaS we are not using VM to host the Flask app and the React app anymore, instead we created 2 separate App Services and deployed the Flask app and the React app to them respectively. Then due to the configuration of the VNET the browsers will be able to access the web page that we deployed.