In Robocorp, we use vault feature to store the credential data. But you can also able to store the credential data in <b>Windows Credential Manager </b> during the development time instead of storing the credential data in local vault environment.

<b>Let's see how it is done !</b>

For the demonstartion purposes, we use the website https://robotsparebinindustries.com/  where the robot log to the website using the credential data stored in windows credential manager.


First Step we had to store the credentials data in windows credential manager under generic credentials as shown below



![Alt text](<https://github.com/nived00015/Working-with-windows-Credential-Manager/blob/main/Credential.png>)
<br>

For retriving the credential data from windows credential manager, a custom library is developed using python module keyring which can be implemented in robocorp.

More about keyring: https://pypi.org/project/keyring/

---------------------------------------------------------------------------


<h2><b>Windows Credential Library</b></h2>

This is the WindowCredential.py file 

```
import keyring
def get_credential(service_name):
    c = keyring.get_credential(service_name,None)
    d1 = dict()
    d1["username"]=c.username
    d1["password"]=c.password
    return d1

def set_credential(service_name, username, password):
    keyring.set_password(service_name, username, password)

    return True
```
In this library, there are two functions . First one is used for retrieving the credential data based on the service name (Internet or network address), Second one is used for setting the password for the particular username based on service name.

-------------------------------------------------------------------------------------------------------------------------


<h2><b>Robot</b></h2>

task.robot 

This robot will retrive the credentials from window credential and logs into the website.

```
*** Settings ***
Documentation   Template robot main suite.
Library         WindowCredential
Library         RPA.Browser
Library         RPA.core.notebook

*** Tasks ***
Logs into robotbinspares websites 
    &{c}=  Get Credential  robocorp
    
    Open Available Browser  http://robotsparebinindustries.com/
    
    Input Text  id:username  ${c}[username]
    
    Input Password  id:password  ${c}[password]
    
    Click Button  //button[@type="submit"]
    
```


```Get Credential``` is provided by ```WindowCredential``` library. <br>
It  takes the internet or network address as input (in this example, input is robocorp as it is the name of internet or network address we specified while creating credentials in windows credentials manager) and output will be like a dictionary format as below. 
 
 ```
 {"username": User Name
 "password": password }
 ```
 
 from the dictionary format output, we are retriving the username and password as per the example by ${c}[username] and ${c}[passowrd] respectively.
 
 <b>Video to demonstarte the working </b>
 

https://user-images.githubusercontent.com/64367090/109547900-fb7e5680-7af1-11eb-99d5-f9808f1a9981.mp4


Simmilary if we need to change password we can use the  ```Set Credential``` keyword provided by the ```WindowCredential```. The format of using the keyword is 

```Set Credential  ServiceName  username   password```

(Where the ServiceName is the name of internet or network address.)

<b>Note:</b> The password argument provided in the keyword is the new password to be changed for given username and ServiceName.





 
 
 


