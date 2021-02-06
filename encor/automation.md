<!---Chapter 28 and 29 for 6.0 Automation--->
# 6.0 Automation

## Application Programming Interface (API)
 - Mechanisms used to communitace with applications and other software.
 - Used to communicate with various components of the network through software.  
- It is possible to use APIs to configure or monitor specific components of a network. 

Representational State Transfer (REST) APIs
- Often referred to as RESTful API.
- Uses HTTP methods to gather and manipulate data. 
- HTTP is a defined structure which makes it consistent in the way it interacts with APIs from different vendors. 

HTTP Functions
| HTTP Function | Action                        |
| ------------- | ----------------------------- |
| GET           | Retrieve/Read resource        |
| POST          | Create new resource           |
| PUT           | Update/Replace a resource     |
| PATCH         | Update/Modify/Append          |
| DELETE        | Remove a resource             | 

HTTP Status Codes
| Success 2xx   | Result/Description  |
| ------------- | ------------------- |
| 200           | OK                  |
| 201           | Created             |
| 202           | Accepted for processing, but has not been completed    |
| 204           | Server fulfilled request but does not return a body       |

| Client Error 4xx | Result/Description  |
| ------------- | ------------------- |
| 400              | Bad Request         |
| 401              | Unauthorized        |
| 403              | Forbidden           |
| 404              | Not Found            | 

| Server Error 5xx | Result/Description |
| ------------- | ------------------- |
| 500              | Bad Request     |
| 501              | Unauthorized      |


- Northbound API
    - Upwards.
    - Communicate from a network controller to its management software.
    - Traffic between the software and the controller should be encrypted using TLS.
- Southbound API
    - Downwards.
    - When you make changes to a switch in the management software of the controller, those changes are pushed down to the device using the Southbound API.

 
## DNA Center APIs
- All data in JSON format.
- Authentication:
    -  HTTP POST used to authenticate. 
    - Uses basic authentication.
    -  URL for getting token to DNA Center API
        > https://sandboxdnac.cisco.com/api/system/v1/auth/token
        - This token can now be used for other API calls towards other components in DNA Center.
        -  The token received should be used in the next API call in the header with the key:
        
        >| **Key** | **Value** |
        >| ------------- | ------------------- |
        >| X-Auth-Token | "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiI1YTU4Y2QzN2UwNWJiY.........." |

## Cisco vManage APIs
Authenticating and getting token for SD-WAN API:
1. URL must target the Authentication API.
2. HTTP POST.
3. Header Content-Type must be application/x-www-form-urlencoded
4. Body must contain username and password.


## YANG Data Models
- Alternative to SNMP MIBs.
- Data models create a uniform way to describe data. 
- Used to describe:
    - What can be configured on a device.
    - Everything that can be monitored on a device.
    - All the administrative actions that can be executed on a device. 
- YANG models use a tree structure and is constructed in a modules. Similar to XML.
    - The modules are hierarchical and contain all the different data and types that make up a YANG device model.
    - The tree structure represents how to reach a specific element of the model, and the elements can be either configurable or not configurable. 
    - Every element has a defined type.
        - E.g. an interface can be configured to be **on** or **off**. However, the operational state cannot be changed. Meaning if the options are **on** or **off**, no other option/value can be entered there, only **on** or **off**. 
- YANG models make a clear distinction between configuration data and state information.

#### YANG Model Example
>General example: [yang-example.yang](https://github.com/yobenajar/notes/tree/main/encor/yang-example.yang)  
>Network example: [yang-example-network-oriented.yang](https://github.com/yobenajar/notes/tree/main/encor/yang-example-network-oriented.yang)


## NETCONF
- Uses YANG data models to commmunicate with devices on the network. 
- NETCONF runs over SSH, TLS and SOAP
- Can distinguish between configuration data operational data, unlike SNMP.
-  NETCONF uses paths to describe resources.
    - E.g. "interfaces/interface/eth0"

**Use cases for NETCONF**
- Collecting the status of specific fields. 
- Changing the configuration of specific fields.
- Taking administrative actions.
- Sending event notifications.
- Backing up and restoring configurations.
- Testing configurations before finalizing the transaction.


| NETCONF Operation | Description |
| ----------------- | ------------------- |
| `<get>`           | Requests running configuration and state information of the device     |
| `<get-config>`    | Requests some or all of the configuration from a datastore     |
| `<edit-config>`   | Edits a configuration datastore by using CRUD operations     |
| `<copy-config>`   | Copies configuration to another datastore      |
| `<delete-config>` | Deletes configuration     |


**Difference between SNMP and NETCONF**
| Feature | SNMP| NETCONF |
| ----------------- | ------------------- | -----------|
| Resources         | OIDs | Paths |
| Data models       | Defined in MIBs | YANG core models |
| Data modeling language  | SMI | YANG |
| Management operations | SNMP  | NETCONF |
| Encoding | BER   |  XML, JSON|
| Transport stack | UDP    | SSH/TCP |


## RESTCONF
- Provides a RESTful API experience while still leveraging the device abstraction capabilities provided by NETCONF. 
- RESTCONF supports the following CRUD operations
    - GET, POST, PUT, DELETE, OPTIONS
- Uses either JSON or XML.


## Embedded Event Manager (EEM)
- Allows you to buld software applets that can automate many tasks. 