# Salesforce


#### Libraries
* Python
    * Bulk API : https://github.com/wingify/salesforce-bulkipy
        * Try to use this over the heroku one. It has more features like proper encoding and support for 2 factor authentication. 
    * REST API : https://github.com/simple-salesforce/simple-salesforce
        * Method for getting the access token
```
import requests
# loginUrl for sandbox is 'https://test.salesforce.com'
def getToken(loginUrl, key, secret, user, pass):
    payload = {
		'grant_type': 'password',
		'client_id': key,
		'client_secret': secret,
		'username': user,
		'password': pass
	}
	r = requests.post("%s/services/oauth2/token" % loginUrl, 
		headers={"Content-Type":"application/x-www-form-urlencoded"},
		data=payload)
		
	return json.loads(r.content)
```

