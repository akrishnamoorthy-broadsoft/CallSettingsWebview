# CallSettingsWebview

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
NOTE : Replace the <IP-Address> referred throughout with the IP address of the machine where the dev environment is to be setup. IP address is to be used when the application has to be accessed from a different machine or a phone while the setup is done in a different machine. Alternatively, 127.0.0.1 can be used when the setup is on the same machine where the application is accessed from.
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
--------------------
Pre-requisites:
--------------------
Install the Visual Studio Code editor from the below link:
                    https://code.visualstudio.com/download
             and depending on your operating system install the appropriate one.
 
Install Eclipse and Tomcat, to modify and run the BWWebProxy web application that is necessary to run the CSW application in dev mode. This application works as a proxy to overcome CORS in the Xsi-Actions application.

Install Node JS from www.nodejs.org and depending on your OS select the appropriate download(The node and npm versions should be at least v6.0 and v3.10.10 respectively or higher. The versions can be checked by executing the commands “>node -v” and “>npm -v” respectively in a command prompt terminal after installation).
Checkout the BWWebProxy java web application.
    Make the below modifications in the file before hosting the application.
        1. In the BWWebProxyServlet.java the application sets the "Access-Control-Allow-Origin" header with the address of the CSW application in dev mode. Search for all the occurrences where this header is set and make sure the IP address of the machine where the CSW application would be run is set there.
                Eg., resp.addHeader("Access-Control-Allow-Origin", "http://<IP-Address>:4200");        
Compile the application with the     
Install and deploy the BWWebProxy web application in tomcat server. Check for the PORT in which tomcat is running and that is to be set in the httpservices.service.ts file later.

Steps to setup the environment :
================================
Check out the CallSettingsWeb code base from the respective SVN location.
           Make the below modifications in the files to run the application in dev mode.
            1. (WebContent/angular2/).angular-cli.json  - In this file the apps.index property value should be index.html
                    "index": "index.html"
            2. (WebContent/angular2/src/)index.html - This file launches the application. The XSP URL, UserId and Password for the user that is to be used in dev mode are to be set in this file.
                Set the "devModeCallSettingsJSON.userId" to the userId that the dev environment will run upon.
                Set the "devModePwd" property with the password for the userId.
                Set the "xsiActionsBaseURL" window variable to the Xsi-Actions URL in the XSP that is to be used for testing.
                Set the "devModeCallSettingsJSON.deviceMobileNo" to the number that should be used for the BroadWorksAnywhere service Mobile identity.
            3. (WebContent/angular2/src/app/AppCommon/)httpservices.service.ts
                Set the "proxyUrl" property with the URL where the BWWebProxy application that is deployed is hosted. Recommended to use the IP address of the machine where the application is hosted.
                    eg., "http://<IP-Address>:8081/BWWebProxy/proxy";

The "index.html" simulates the input from a UC application. The "devModeCallSettingsJSON" JSON object simulates the UC One input JSON. 
The customizedTexts JSON object is the one used by the application for the different text and label in the application in dev mode.
To simulate branding, branding.css is to be modified. Any changes made to branding has to be carried forward to the customStyle.css.template with the placeholders as that is to be replaced with.
Now going into the “angular2” folder, inside “WebContent” folder, under the CallSettingsWeb checked out folder, open a command prompt terminal and execute the following command:
                         >npm install
                 This will install all the necessary packages and modules for running the angular codes listed in the “package.json” file. A folder named “node_modules” will be created.
                 Note: Proper internet connectivity is required for this step.
In the same command prompt terminal within the “angular2” folder execute this command:
                      >ng serve
                  This will compile the code and run it in the local machine, which can be viewed by hitting http://<IP-Address>:4200 in the browser.
                   Note: Tomcat server should be running in the host machine with the BWWebProxy server hosted in it. Also, the BWWebProxy application should point to the “localhost:4200” for all server calls (Should be modified in the BWWebProxy source code.)
To host the application in the local machine and to access it from other devices, we have to get the current ipv4 address of the hosting machine and in the same command prompt terminal within the “angular2” folder execute the following command:
                   >ng serve - -host <ipv4 address>      (Note: The consecutive ‘-‘ has no spaces in between.)
                  After the successful execution of the above command, the web application hosted in the local machine can be accessed by hitting “<ipv4 address of the local machine>:4200”.
To edit the code in Visual Studio Code editor, open it and add the project folder to it.


----------------------------------------------------------------------------------------------------------------------------------------
Build & deploy the CallSettingsWeb application
----------------------------------------------------------------------------------------------------------------------------------------
Inside the “angular2” folder, inside “WebContent” folder, under the CallSettingsWeb checked out folder, open a command prompt terminal and execute (if not already done) the following command:
                 >npm install
                   This will install all the necessary packages and modules for running the angular codes listed in the “package.json” file. A folder named “node_modules” will be created.
                   Note: Proper internet connectivity is required for this step.
Installing Apache Ant, if not already installed.
In eclipse, import the project folder which is to be built, the built version can be updated in the “build.properties” file inside the “build” folder.
(WebContent/angular2/).angular-cli.json  - In this file the apps.index property value should be "../cswindex.jsp"
                    "index": "../cswindex.jsp"
Ensure that the above entry is present. If this is not set, the war deployed in XSP is not going to open the application.
Opening a command prompt terminal in the “build” folder execute the following command:
              >ant
                      This builds the “BWCallSetingsWeb_<version_number>.war” file inside the “dist’ folder created in the process under “build”.
This “BWCallSetingsWeb_<version_number>.war” is copied/moved to the XSP and installed, activated and deployed.
