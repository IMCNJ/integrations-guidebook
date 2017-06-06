# Eloqua



#### Links
* [pyeloqua](https://github.com/colemanja91/pyeloqua) is not bad. Doing it with a requests library has proved to be easier. 
* [API Documentation](http://docs.oracle.com/cloud/latest/marketingcs_gs/OMCAC/)


#### REST API General info
* Search fields/terms
	* <field_name><operation><value>
	* You can use wild charater *. Ex: `search=EmailAddress1=steve*`
	* You can combine multiple searches in one parameter. Ex: `search=endAt>1465224632 endAt<1465829432 currentStatus=1`

#### Bulk API

#### CDOs 
* Find CDO id (parent id)
`GET api/Bulk/2.0/customObjects?q=name='search_term_that_can_include_a_wildcar*`

* Get fields for CDO
`GET api/Bulk/2.0/customObjects/{parentId}/fields`

* Search for instances 
`api/REST/2.0/data/customObject/{parentId}/instances?search=FieldName=value`
	* Value does not get encased in anything
	* field name has to come from a name of one of the fields returned when pull the fields from the parent ID

#### Mass deleting assets
* Sometimes in test instances you want to mass delete assets. Here is a small python script to do this.
```
import requests
def deleteAssets(ids, url, username, company, password):
	""" Deletes given ids on asset type defined in the url

		ex : url
		https://secure.p01.eloqua.com/api/REST/1.0/assets/email/'
		Must have ending / so that the id can be added to the end
	"""
	notdeleted = []
	for asset_id in ids:
		deleteIdUrl = url + str(asset_id)
		response = requests.delete(
			deleteIdUrl, 
			auth=HTTPBasicAuth('%s\%s' % (company , user), password))
		if response.status_code == 200:
			print str(asset_id) + " deleted"
		else:
			print str(asset_id) + " could not be deleted"
			notdeleted.append(asset_id)

	print 'not deleted: ' + str(notdeleted)
```