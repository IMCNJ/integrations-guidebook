# Eloqua



#### Links
* [pyeloqua](https://github.com/colemanja91/pyeloqua) is not bad. Doing it with a requests library has proved to be easier. 
* [API Documentation](http://docs.oracle.com/cloud/latest/marketingcs_gs/OMCAC/)


#### REST API General info
* Search fields/terms
	* <field_name><operation><value>
	* Example : `search=endAt>1465224632 endAt<1465829432 currentStatus=1`



#### Bulk API
* CDOS


#### CDOs 
* Find CDO id (parent id)
`GET api/Bulk/2.0/customObjects?q=name='MSD_Cascadion Contacts to SF*`

* Get fields for CDO
`GET api/Bulk/2.0/customObjects/{parentId}/fields`

* Search for instances 
`api/REST/2.0/data/customObject/{parentId}/instances?search=FieldName=value`
	* Value does not get encased in anything
	* field name has to come from a name of one of the fields returned when pull the fields from the parent ID

