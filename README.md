# pyclearpass
## Aruba ClearPass V6.11 SDK
Aruba ClearPass SDK has been developed in Python v3.9 to utilise the full functionality of the Aruba ClearPass REST API environment. Each available REST API command is available for use in this module.

This package has been uploaded to https://pypi.org/ and is also available to install via "https://github.com/aruba/pyclearpass. Installation instructions are provided below. 
## Available API Categories 
The following describes the available top level functionality of the ClearPass API available within this Python Package. 
- Operations
- Certificate Authority
- Endpoint Visibility 
- Enforcement Profile
- Global Server Configuration
- Guest Actions
- Guest Configuration
- Identities
- Insights
- Integrations
- Local Server Configuration 
- Logs
- Platform Certificates
- Policy Elements
- Session Controls
- Tools and Utilities

_This package comes without any warranties and should be used at your own risk._

## ClearPass Server Readiness
These steps list what is required on the ClearPass server:
1. Make sure you have the API Service enabled within Services. You may use the template to help you do this. 
2. Create a new API Client within the ClearPass Guest Portal 
a. client id = demo, 
b. enabled, Operating Mode =Rest API, 
c. Operator Profile = Pick one with apporpiate permission level or make a new one,  
d. Grant Type = client credentials)
e. Acess Token Lifetime: 8 Hours
3. Optional but preferred, a valid SSL cerfificate

If you need information, refer to the ClearPass configuration documentation for the API account -
https://developer.arubanetworks.com/aruba-cppm/docs/clearpass-configuration  

# Python Requirements  
Ensure Python v3.9 or greater is installed on your operating system
# Package Installation  
#### Method 1 - Installing Package from PyPi
Run the following in a command line terminal to install the pip package - ```pip3 install pyclearpass``` or ```pip install pyclearpass```. This may vary between Operating Systems. 

#### Method 2 - Installing Package from Github (not using Git.exe)
1. Click into the Aruba Github Repository where the latest version of pyclearpass is located 
2. Click Code (in green) and Download to Zip
3. Extract the zip file into a directory
4. Go into a command line terminal and change directory (cd) into the folder where you extracted the zipped file and then down one child folder. The folder contents should pyclearpass (FOLDER), LICENCE, pyproject.toml and README.md   
5. In your command line terminal type ```python3 -m build``` or ```python -m build```. This will create a folder called dist with a file containing a .gz extension. 
6. Run the following in a command line terminal to install the pip package - ```pip3 install pathtozip.gz``` or ```pip install pathtozip.gz```. This may vary between Operating Systems.

#### Method 3 - Installing Package from Github (using Git.exe)
1. Install Git for your Operating System from https://git-scm.com/download
2. Run the following in a command line terminal to install the pip package - ```pip3 install git+https://github.com/aruba/pyclearpass``` or ```pip install git+https://github.com/aruba/pyclearpass```. This may vary between Operating Systems.

## Inital Usage Instructions
Within your Python favourite IDE enivronment, create an import reference
```
from pyclearpass import *
```
Create a object to login into ClearPass. The login object needs to be passed to use any function within the ClearPass API.
An example below shows how to create the login object.
>Note - The login object will contain the APIToken once any function has been used. 
It grabs it once for the session and uses the same token through the execution of the rest of the script. 

```
login = ClearPassAPILogin(server="https://yourserver.network.local:443/api",granttype="client_credentials",
clientsecret="clientsecret", clientid="clientid", verifySSL=False)
```
Find an API you want to use, by prefixing  ```api```  in your IDE and intellisense will show the available APIs available. Each of the top level API category names are available as a module. Once you have chosen a specifc API to use, for example apiPolicyElements, it will show you the available methods if you suffix a . to the command - ```apiPolicyElements.```

The example below prints the roles available within the clearpass server.
```	
print(apiPolicyElements.getRole(login)) 
```
By default, the example above to return the roles available within the clearpass server will only show the first 25 roles. If you want to view more, you have to adjust the limit. Placing your cursor over the .getRole will usually show you help about the method. 
```
print(apiPolicyElements.getRole(login, limit=100))
```
## Help 
Once you have written a specific API  ```apiName.FunctionName(```, placing your cursor over the command will show you help for the function and what the required parameters are (example is Visual Studio Code). The first parameter is always login. 
You may also read the help for the function by calling ```help(apiName.FunctionName)```. Each function contains a help section on how to use it. 
## Python Package Upgrade Instructions
Once an update is available on the Python PyPi repository, you may upgrade your release by completing the following in a command line terminal - ```pip3 install pyclearpass --upgrade```
## Uninstall Package Package
To remove the Python pyclearpass package, type the following command into a command line terminal - ```pip3 uninstall pyclearpass ``` or ```pip uninstall pyclearpass ```

## Further Usage Examples
#### Get Local Server Configuration 
```
LSCGCS = apiLocalServerConfiguration.getClusterServer(login)
print(json.dumps(LSCGCS['_embedded']['items'],indent=1))
```
#### Get Total End Point Count 
```
IGEP = apiIdentities.getEndpoint(login, calculate_count='true')
print("Total MACs in Table: "+str(IGEP['count']))
```
#### Get Insight Device Details
```
print(apiLogs.getInsightEndpointIpByIp(login,ip="192.168.0.99"))
```

#### Get list of Admin Users
```
AU = apiGlobalServerConfiguration.getAdminUser(login)
for users in AU['_embedded']['items']:
  print(users)
```

#### Add New Endpoint
```
newEndPoint = {
  "mac_address": "11:22:33:44:55:66",
  "description": "Demo EndPoint 1",
  "status": "Known"
}
print(apiIdentities.newEndpoint(login,body=newEndPoint))
```
#### Add New Role
```
role={"name": "Test1","description": "Test role made using the API Package in Python"}
print(apiPolicyElements.newRole(login,body=role))
```

#### Delete Role
```
print(apiPolicyElements.deleteRoleNameByName(login,name='Demo'))
```
