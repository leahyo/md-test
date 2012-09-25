# API Specifications for Block Storage


**Author(s):** *[Put in Author Names]*  
**Date:**  *[Date]*  
**Document Version:** *[Put in the current document version]*


---


# 1. Overview

The Bock service provides on demand block storage to Nova VM's




## 1.1 API Maturity Level

**Maturity Level**: *Exploratory* Private-beta ready

**Version API Status**: *BETA*


---


# 2. Architecture View


## 2.1 Overview
*[References to architectural details of the service.]*

## 2.2 Conceptual/Logical Architecture View
*[Describe the logical components of the system and their responsibilities]*

## 2.3 Infrastructure Architecture View
*[Describe how the API fits into the overall HPCS Infrastructure]*

## 2.4 Entity Relationship Diagram
*[Describe the relationships between the various entities (resources) involved in the API]*


---

# 3. Account-level View
*[Describe the relationship of the API with respect to the accounts, groups, tenants, regions, availability zones etc.]*


## 3.1 Accounts
*[Describe the structure of the user accounts, groups and tenants. Currently this might be described separately in context of Control Services, but in future each service API needs to state their usage. In future CS might support complex group hierarchies, enterprise account structures while there maybe a phased adoption by individual service APIs]*

## 3.2 Regions and Availability Zones
*[Describe the availability of the service API in different regions and availability zones. State plans for future expansion as well.]*

**Region(s)**: region-a

**Availability Zone(s)**: az-1, az-2, az-3 

**Future Expansion**: region-b


## 3.3 Service Catalog
*[Describe if the service API is exposed via the service catalog. Reference the fragment of the service catalog showing the structure.]*

The service is exposed in the service catalog, as shown in the following fragment:


```
{
      "name": "Block Storage",
      "type": "volume",
      "endpoints": [
        {
          "tenantId": "<tenant_id>",
          "publicURL": "https:\/\/az-1.region-a.geo-1.compute.hpcloudsvc.com\/v1.1\/<tenant_id>",
          "region": "az-1.region-a.geo-1",
          "versionId": "1.1",
          "versionInfo": "https:\/\/az-1.region-a.geo-1.compute.hpcloudsvc.com\/v1.1\/",
          "versionList": "https:\/\/az-1.region-a.geo-1.compute.hpcloudsvc.com"
        },
        {
          "tenantId": "<tenant_id>",
          "publicURL": "https:\/\/az-3.region-a.geo-1.compute.hpcloudsvc.com\/v1.1\/<tenant_id>",
          "region": "az-3.region-a.geo-1",
          "versionId": "1.1",
          "versionInfo": "https:\/\/az-3.region-a.geo-1.compute.hpcloudsvc.com\/v1.1\/",
          "versionList": "https:\/\/az-3.region-a.geo-1.compute.hpcloudsvc.com"
        },
        {
          "tenantId": "<tenant_id>",
          "publicURL": "https:\/\/az-2.region-a.geo-1.compute.hpcloudsvc.com\/v1.1\/<tenant_id>",
          "region": "az-2.region-a.geo-1",
          "versionId": "1.1",
          "versionInfo": "https:\/\/az-2.region-a.geo-1.compute.hpcloudsvc.com\/v1.1\/",
          "versionList": "https:\/\/az-2.region-a.geo-1.compute.hpcloudsvc.com"
        }
      ]
}
```

---



# 4. REST API Specifications
*[Describe the API specifications, namely the API operations, and its details, documenting the naming conventions, request and response formats, media type support, status codes, error conditions, rate limits, quota limits, and specific business rules.]*

## 4.1 Service API Operations


**Host**: https://az-1.region-a.geo-1.compute.hpcloudsvc.com

**Base URI**: [Host]/v1.1/<tenant_id>

**Admin URI**: N/A

| Resource | Operation            | HTTP Method | Path                   | JSON/XML Support? | Privilege Level |
| :------- | :------------------- | :---------- | :--------------------- | :---------------- | :-------------: |
| **Versions** | Version information | GET     | [Host]/v1.1/           | Y/**N**  |  |
|              | Version list        | GET     | [Host]/                | Y/**N**  |  |
| **Volumes**  | List volumes        | GET     | [BaseUri]/os-volumes   | Y/**N**  |  |
|              | Create new volume   | POST    | [BaseUri]/os-volumes   | Y/**N**  |  |


## 4.2 Common Request Headers
*[List the common response headers i.e. X-Auth-Token, Content-Type, Content-Length, Date etc.]*

## 4.3 Common Response Headers
*[List the common response headers i.e. Content-Type, Content-Length, Connection, Date, ETag, Server, etc. ]*

## 4.4 Service API Operation Details
*[The following section, enumerates each resource and describes each of its API calls as listed in the Service API Operations section, documenting the naming conventions, request and response formats, status codes, error conditions, rate limits, quota limits, and specific business rules.]*

### 4.4.1 [Resource]
Allows you to manage volumes and snapshots that can be used with the Compute API through an extension to the API.

**Status Lifecycle**

N/A

**Rate Limits**

N/A

**Quota Limits**

N/A

**Business Rules**

None.


#### 4.4.1.1 List volumes
#### GET /v1.1/\<Tennant Id\>/os-volumes

Get a list of the volumes associated with the specified tennant id. If 
the attribute 'ImageRef' has a non empty string as its value then the volume 
is a bootable volume, and the value of 'ImageRef' is the Glance image id, 
this feature has not yet been implemented. The attribute 'availabilityZone' 
does not correspond to HP Cloud availability zones.

**Request Data**

None

**URL Parameters**

None

**Data Parameters**

This call does not require a request body


**Success Response**

**Status Code**

200 - OK

**Response Data**

JSON

```
{
    "volumes": [
        {
            "status": "available",
            "displayDescription": null,
            "availabilityZone": "nova-zone",
            "displayName": "VOLUME-4-929012.2012-08-20 04:57:53",
            "attachments": [{}],
            "volumeType": "None",
            "snapshotId": "",
            "imageRef": "",
            "size": 5,
            "id": 33688,
            "createdAt": "2012-08-20 04:58:18",
            "metadata": {"contents": "junk"}
        },
        {
            "status": "available",
            "displayDescription": "Test volume",
            "availabilityZone": "nova-zone",
            "displayName": "Another Volume",
            "attachments": [{}],
            "volumeType": "None",
            "snapshotId": null,
            "imageRef": null,
            "size": 1,
            "id": 34186,
            "createdAt": "2012-08-21 10:58:30",
            "metadata": {}
        }
    ]
}
```

**Error Response**

There are no error status' specific to the volumes API

**Status Code**

500 - Internal Server Error

**Response Data**

JSON

```
{"cloudServersFault": {"message": "Server Error, please try again later.", "code": 500}}
```

**Status Code**

401 - Unauthorized

**Response Data**

JSON

'''
{"cloudServersFault": {"message": "This server could not verify that you are authorized to access the document you requested. Either you supplied the wrong credentials (e.g., bad password), or your browser does not understand how to supply the credentials required.", "code": 401}}
'''

**Curl Example**

```
curl -i -H "X-Auth-Token: <Auth_Token>" [BaseUri]/os-volumes
```

**Additional Notes**

None


---
#### 4.4.1.2 List the details for a specified volume.
#### GET /v1.1/\<Tennant Id\>/os-volumes/\<Volume Id\>

Returns the details for the volume specified by *Volume Id*

**Request Data**

None

**URL Parameters**

None

**Data Parameters**

None

This call does not require a request body



**Success Response**

**Status Code**

200 - OK

**Response Data**

JSON

```
{
    "volume": [
        {
            "status": "available",
            "displayDescription": null,
            "availabilityZone": "nova-zone",
            "displayName": "VOLUME-4-929012.2012-08-20 04:57:53",
            "attachments": [{}],
            "volumeType": "None",
            "snapshotId": "",
            "imageRef": "",
            "size": 5,
            "id": 33688,
            "createdAt": "2012-08-20 04:58:18",
            "metadata": {"contents": "junk"}
        }
}
```


**Error Response**

**Status Code**

500 - Internal Server Error

**Response Data**

JSON

```
{"cloudServersFault": {"message": "Server Error, please try again later.", "code": 500}}
```
**Status Code**

404 - Not Found

**Response Data**

JSON

```
{"itemNotFound": {"message": "The resource could not be found.", "code": 404}}
```


**Curl Example**

```
curl -i -H "X-Auth-Token: <Auth_Token>" [BaseUri]/os-volumes/[VolumeId]
```

**Additional Notes**

None


---
#### 4.4.1.3 Create a volume.
#### POST /1.1/\<Tennant Id\>/os-volumes

Create a new volume for the specified tenant id. If the snapshot_id 
attribute is not null then the volume is created as a copy of the 
specified snapshot. If the imageRef parameter is not null then a bootable 
volume is created. The attribute 'availabilityZone' does not correspond 
to HP Cloud availability zones.

The attribute **imageRef** has not yet been implemented.

The request is invalid if both the snapshot_id and the imageRef parameters 
are specified and are not null.

**Request Data**

The request body specifies the size and some display information for the 
volume to be created in attribute value pairs.

**URL Parameters**

None

**Data Parameters**

* *snapshot_id* (Optional) - String - The id of a snapshot from which to create the volume
* *display_name* (Optional) - String - User defined name for the volume
* *display_description* (Optional) - String - User supplied description for the volume
* *imageRef* (Optional) - String - The id of a Glance image from which to create the volume, this parameter is not yet supported
* *metadata* (Optional) - String - User supplied data
* *availability_zone* (Optional) - String - The nova zone
* *volume_type* (Optional) - String - The volume type, this parameter is currently ignored
* *size* (Required) - Integer - The size of the volume to create, in GBytes


JSON

```
{
    "volume": {
        "snapshot_id": null,
        "display_name": "Test Volume 3",
        "display_description": "Test volume",
        "size": 1,
        "metadata": {"contents": "junk"},
        "availability_zone": "nova-zone",
        "volume_type": "None",
        "imageRef": "3"
    }
}
```

**Success Response**

**Status Code**

200 - OK

**Response Data**

JSON

```

{
    "volume": {
        "status": "creating",
        "displayDescription": "Test volume",
        "availabilityZone": "nova",
        "displayName": "Test Volume 3",
        "attachments": [{}],
        "volumeType": "None",
        "snapshotId": null,
        "image_id": "3"
        "size": 1,
        "id": "2343",
        "createdAt": "2012-08-21 13:55:48",
        "metadata": {}
    }
}
```

**Error Response**

**Status Code**

500 - Internal Server Error

**Response Data**

JSON

```
{"cloudServersFault": {"message": "Server Error, please try again later.", "code": 500}}
```

**Status Code**

413 - Internal Server Error

**Response Data**

JSON

```
{"OverLimit": {"message": "Volume quota exceeded. You cannot create a volume of size 10G.", "code": 413}}
```


**Curl Example**

```
curl -i -H "X-Auth-Token: <Auth_Token>" [BaseUri][path]
```

**Additional Notes**

*[Specify any inconsistencies, ambiguities, issues, commentary or discussion relevant to the call.]*



---



#### 4.4.1.1 [Short description of the method call]
#### [HTTP Verb: GET, POST, DELETE, PUT] [path only, no root path]

*[Description about the method call]*

**Request Data**

*[Specify all the required/optional url and data parameters for the given method call.]*

**URL Parameters**

*[Pagination concepts can be described here, i.e. marker, limit, count etc. Filtering concepts can be described as well i.e. prefix, delimiter etc.]*

* *[name_of_attribute]* - [data type] - [description of the attribute]
* *[name_of_attribute]* - [data type] - [description of the attribute]
* *[name_of_attribute]* (Optional) - [data type] - [description of the attribute]

**Data Parameters**

*[List all the attributes that comprises the data structure]*

* *[name_of_attribute]* - [data type] - [description of the attribute]
* *[name_of_attribute]* - [data type] - [description of the attribute]
* *[name_of_attribute]* (Optional) - [data type] - [description of the attribute]

*[Either put 'This call does not require a request body' or include JSON/XML request data structure]*


JSON

```
{json data structure here}
```

XML

```
<xml data structure here>
```

Optional:

JSON

```
{json data structure here}
```

XML

```
<xml data structure here>
```

**Success Response**

*[Specify the status code and any content that is returned.]*

**Status Code**

200 - OK

**Response Data**

*[Either put 'This call does not require a request body' or include JSON/XML response data structure]*

JSON

```
{json data structure here}
```

XML

```
<xml data structure here>
```

**Error Response**

*[Enumerate all the possible error status codes and any content that is returned.]*

**Status Code**

500 - Internal Server Error

**Response Data**

JSON

```
{"cloudServersFault": {"message": "Server Error, please try again later.", "code": 500}}
```

XML

```
<xml data structure here>
```

**Curl Example**

```
curl -i -H "X-Auth-Token: <Auth_Token>" [BaseUri][path]
```

**Additional Notes**

*[Specify any inconsistencies, ambiguities, issues, commentary or discussion relevant to the call.]*


---

# 5. Additional References

## 5.1 Resources

**Wiki Page**: [Link to Wiki page]

**Code Repo**:  [Link to the internal Github repo]

**API Lead Contact**: [Name of contact]

---

# 6. Glossary

[Put down definitions of terms and items that need explanation.]

---


