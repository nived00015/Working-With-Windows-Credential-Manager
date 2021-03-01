In Robocorp, we use vault feature to store the credential data. But you can also able to store the credential data in <b>Windows Credential Manager </b> during the development time instead of storing the credential data in local vault environment.

<b>Let's see how it is done !</b>

For the demonstartion purposes, we use the website https://robotsparebinindustries.com/  where the robot log to the website using the credential data stored in windows credential manager.


First Step we had to store the credentials data in windows credential manager under generic credentials as shown below



![Alt text](<https://github.com/nived00015/Working-with-windows-Credential-Manager/blob/main/Credential.png>)
<br>

For retriving the credential data from windows credential manager, a custom library is developed using python module keyring which can be implemented in robocorp.

More about keyring: https://pypi.org/project/keyring/

----
<h3><b>Windows Credential Library</b></h3>

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


