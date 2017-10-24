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


#### Undocumented features

 * When pulling a CDO, using the undocumented 2.0 enpoint gives you the record count of what is in the CDO 
 `GET /api/REST/2.0/assets/customObject/{parentId}`
 instead of...
 `GET /api/REST/1.0/assets/customObject/{parentId}`



#### Parsing CDO json data to csv 
* Here is a quite python script to output a csv file from an input of cdo data aquired from the api. 

```
import json 

fname = raw_input("Json file: ").rstrip()
with open(fname , 'r') as f:
	json_obj = json.load(f)
with open('out_json_to_csv_cdo.csv', 'w') as out:
	field_headers=[]
	for field in json_obj['elements'][0]['fieldValues']:
		out.write( field['id'] + ',')
		field_headers.append(field['id'])
	out.write( '\n')
	for entry in json_obj['elements']:
		for field_header in field_headers:
			for field in entry['fieldValues']:
				if field['id'] == field_header:
					val = field.get('value')
					break
			if val == None:
				val = ""
			out.write( val + ',')
		out.write( '\n')
```
