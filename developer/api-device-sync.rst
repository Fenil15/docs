.. _ref-device-sync:

Sync
====
This family of device APIs provided by dronahq.js allows a micro-app an easy way to submit data in background via http requests. 

It has the following method(s) -

	- :ref:`upload() <upload>`
	- :ref:`refresh() <refresh>`
	- :ref:`getpendinguploadcount() <getpendinguploadcount>`
	- :ref:`removeRequest() <removeRequest>`
	- :ref:`getPurgeData() <getPurgeData>`
	- :ref:`triggerSync() <triggerSync>`
	- :ref:`removeRequest() <removeRequest>`
	- :ref:`getRequestDetails() <getRequestDetails>`
	- :ref:`getChannelDB() <get-channel-db>`
	- :ref:`getAppDB() <get-app-db>`
	- :ref:`getDBDetails() <get-db-details>`
	- :ref:`getDocument() <get-document>`

It also provide the following event(s) -

	- :ref:`uploadcomplete <uploadcomplete>`

.. _upload:

upload()
--------

This method uploads JSON data to a remote URL.  Optionally you can also upload an image file using this request. The JSON data is sent in the request body. By invoking this method, the micro-app will keep trying the data upload until the server returns an http 2XX response. For failed requests, the app will keep trying to upload the data in every 1 hour.

Please note that calling this method only submits the request to the Container App. Actual upload happens when the sync service is triggered the next time. You can call the refresh method to trigger the sync service.


**REQUEST PARAMETERS**

+--------------+----------+-----------------------------------------+
|Parameter     |Type      |Description                              |
+==============+==========+=========================================+
|remoteurl     |string    |Required. http(s) URL of your API where  |
|              |          |the data should be submitted             |
+--------------+----------+-----------------------------------------+
|requestmethod |string    |Required. HTTP method type for the       |
|              |          |request. Supported values are            |
|              |          |'POST' & 'PUT'                           |
+--------------+----------+-----------------------------------------+
|requestdata   |object    |*Required* A valid JSON object which will|
|              |          |be sent in the request body.             |
+--------------+----------+-----------------------------------------+
|imagefilepath |string    |*Optional* File Path URI for the image   |
|              |          |which should also get uploaded along with|
|              |          |the request.                             |
+--------------+----------+-----------------------------------------+
|options       |object    |*Optional* A object with more request    |
|              |          |parameters like header & timeout.        |
+--------------+----------+-----------------------------------------+
|fnsuccess     |function  |*Optional* Callback function which will  |
|              |          |be called after request has been added to|
|              |          |the queue.                               |
+--------------+----------+-----------------------------------------+
|fnerror       |function  |*Optional* Callback function which will  |
|              |          |be called when request could not be added|
|              |          |to the queue.                            |
+--------------+----------+-----------------------------------------+

.. code:: javascript

	var remUrl = 'https://yourserver.com/api/users/134/action/updatedesignation';
	var dronaHQReqMethod = 'POST';
	var dronaHQReqData = {designation_code : 'X601'};
	var reqImgURI = '';
	var headers = [{ Authorization: 'Auth a2FuY2hhbkBkcm9uYW1vYmlsZS5jb206bWFpbEAxMjM0' }];

	var options = {};
	options.header = headers;
            
	DronaHQ.sync.upload(remURL, dronaHQReqMethod, dronaHQReqData, reqImgURI, options);

.. _refresh:

refresh()
---------

Calling this method will wake up the sync service and triggers any pending upload.

.. code:: javascript
	
	//refreshType = 'upload' for submitting any pending uploads

	DronaHQ.sync.refresh(refreshType);
	
.. _getpendinguploadcount:

getpendinguploadcount()
------------------------

This method gives a count of all the pending upload requests.

.. code:: javascript

	DronaHQ.sync.getPendingUploadCount(function(count){}, function(e){});

.. _uploadcomplete:

uploadcomplete
--------------

.. code:: javascript

	DronaHQ.sync.uploadcomplete 


This event is triggered whenever the sync service has finished processing all pending requests, even if a few requests have failed to complete successfully. The failed requests are retried next time the service runs.

.. code:: javascript
	
	document.addEventListener('dronahq.sync.uploadcomplete', function(){
		//Refresh task is complete.
	});

.. _removeRequest:


removeRequest()
---------------

This method allows users to remove offline request, User must provide list of offline request ID in array format.(received while creating new offline sync request)

.. code:: javascript

	DronaHQ.sync.removeRequest([id1,id2]);

Response for the request is 

+------------------------------------------------------------+------+
| Values	                                                 | Code |
+============================================================+======+
| SYNC_DELETE_ERROR_CODE_FAILURE                             | 0    |
+------------------------------------------------------------+------+
| SYNC_DELETE_ERROR_CODE_SUCCESS                             | 1    |
+------------------------------------------------------------+------+
| SYNC_DELETE_ERROR_CODE_FAILURE_INVALID_ID                  | 2    |
+------------------------------------------------------------+------+
| SYNC_DELETE_ERROR_CODE_FAILURE_ALREADY_DELETED             | 3    |
+------------------------------------------------------------+------+
| SYNC_DELETE_ERROR_CODE_FAILURE_INVALID_UNAUTHORIZED_ACCESS | 4    |
+------------------------------------------------------------+------+


.. _getPurgeData:

getPurgeData()
--------

This method will return details of all the deleted or failed requests.

.. code:: javascript

	DronaHQ.sync.getPurgeData();

Response will be an array of JSON Objects.
Each Object contains keys as follows:

+---------------+------------+-------------------------------------------------+
|Parameter      |Type        |Description                                      |
+===============+============+=================================================+
|data           |JSONObject  |JSONobject which was sent in the request body    |
+---------------+------------+-------------------------------------------------+
|remote_url     |string      |URL of the API                                   |
+---------------+------------+-------------------------------------------------+
|request_method |string      |HTTP method type of the request (POST/PUT/GET)   |
+---------------+------------+-------------------------------------------------+
|file_path      |string      |File Path URI of the image if sent in the request|
+---------------+------------+-------------------------------------------------+
|request_header |JSONArray   |Headers of the API request                       |
+---------------+------------+-------------------------------------------------+
|time_out       |integer     |Time limit for timeout                           |
+---------------+------------+-------------------------------------------------+
|ttl            |integer     |Time to live of request to be retired with       |
|               |            |expiry time                                      |
+---------------+------------+-------------------------------------------------+
|id             |integer     |Microapp id which requested the upload           |
+---------------+------------+-------------------------------------------------+
|is_completed   |boolean     |Is request saved in server or pending            |
+---------------+------------+-------------------------------------------------+
|is_delete      |boolean     |Has request id been auto deleted                 |
+---------------+------------+-------------------------------------------------+
|is_corrupted   |boolean     |Does file exist or auto purged or file is renamed|
|               |            |or moved to some other location, Default is False|
+---------------+------------+-------------------------------------------------+
|response       |string      |Api response from the server                     |
+---------------+------------+-------------------------------------------------+


.. _triggerSync:

triggerSync()
--------

This method will wake up the sync service and trigger any pending upload or download requests.

.. code:: javascript

	DronaHQ.sync.triggerSync();


.. _removeRequest:

removeRequest()
---------------

This method allows users to remove already added requests, 
User must provide list of offline request IDs (which were received while creating offline sync requests) in jsonarray format

.. code:: javascript

	DronaHQ.sync.removeRequest([id1,id2]);

Response of the request is an integer code referring to following values:

+------------------------------------------------------------+------+
| Values                                                     | Code |
+============================================================+======+
| SYNC_DELETE_ERROR_CODE_FAILURE                             |  0   |
+------------------------------------------------------------+------+
| SYNC_DELETE_ERROR_CODE_SUCCESS                             |  1   |
+------------------------------------------------------------+------+
| SYNC_DELETE_ERROR_CODE_FAILURE_INVALID_ID                  |  2   |
+------------------------------------------------------------+------+
| SYNC_DELETE_ERROR_CODE_FAILURE_ALREADY_DELETED             |  3   |
+------------------------------------------------------------+------+
| SYNC_DELETE_ERROR_CODE_FAILURE_INVALID_UNAUTHORIZED_ACCESS |  4   |
+------------------------------------------------------------+------+


.. _getRequestDetails:

getRequestDetails()
---------------

This method returns details of the request as per request ID, User must provide request ID 
(received while creating new offline sync request) as a parameter.

.. code:: javascript

	DronaHQ.sync.getRequestDetails(requestID);

**REQUEST PARAMETERS**

+--------------+----------+-----------------------------------------+
|Parameter     |Type      |Description                              |
+==============+==========+=========================================+
|requestID     |string    |Id received while creating the offline   |
|              |          |sync request                             |
+--------------+----------+-----------------------------------------+

Response of the request is a jsonobject which contains all the keys as follows:

+---------------+------------+-------------------------------------------------+
|Parameter      |Type        |Description                                      |
+===============+============+=================================================+
|data           |JSONObject  |JSONobject which was sent in the request body    |
+---------------+------------+-------------------------------------------------+
|remote_url     |string      |URL of the API                                   |
+---------------+------------+-------------------------------------------------+
|request_method |string      |HTTP method type of the request (POST/PUT/GET)   |
+---------------+------------+-------------------------------------------------+
|file_path      |string      |File Path URI of the image if sent in the request|
+---------------+------------+-------------------------------------------------+
|request_header |JSONArray   |Headers of the API request                       |
+---------------+------------+-------------------------------------------------+
|time_out       |integer     |Time limit for timeout                           |
+---------------+------------+-------------------------------------------------+
|ttl            |integer     |Time to live of request to be retired with       |
|               |            |expiry time                                      |
+---------------+------------+-------------------------------------------------+
|id             |integer     |Microapp id which requested the upload           |
+---------------+------------+-------------------------------------------------+
|is_completed   |boolean     |Is request saved in server or pending            |
+---------------+------------+-------------------------------------------------+
|is_delete      |boolean     |Has request id been auto deleted                 |
+---------------+------------+-------------------------------------------------+
|is_corrupted   |boolean     |Does file exist or auto purged or file is renamed|
|               |            |or moved to some other location, Default is False|
+---------------+------------+-------------------------------------------------+
|response       |string      |Api response from the server                     |
+---------------+------------+-------------------------------------------------+


.. _get-channel-db:

getChannelDB()
--------------

**DEPRECATED**

Please refer :ref:`getDocument() <get-document>` as a alternative of this method.
This method is related to CouchDB.
In order to get all channel specific document i.e pluginid '0' call this method.

.. code:: javascript

	DronaHQ.sync.getChannelDB();

.. _get-app-db:

getAppDB()
----------

**DEPRECATED**

Please refer :ref:`getDocument() <get-document>` as a alternative of this method.
This method is related to CouchDB.
In order to get all microapp specific document i.e pluginid call this method.

.. code:: javascript

	DronaHQ.sync.getAppDB();

.. _get-db-details:

getDBDetails()
--------------

Returns CouchDB(Server details) and environment(Dev, Beta, Prod) of the micro app.

.. code:: javascript

	DronaHQ.sync.getDBDetails();

.. _get-document:

getDocument()
-------------

GetDocument method can be used to for :ref:`getChannelDB() <get-channel-db>`, :ref:`getAppDB() <get-app-db>`.
Retrieve data based on the query(Mango Query) passed by the user.
documentName, query, apikey,type, filterMode, isChannelDoc, skip, limit)

Following are the arguments required to call the method

+-------------+----------+-----------------+-------------+--------------------------------------+
| Parameter   | Type     | Options         | DefaultValue| Description                          |
+=============+==========+=================+=============+======================================+
| documentName| String   | -               | ""          | In couch document 'table_name' key   |
|             |          |                 |             | is the document name.                |      
+-------------+----------+-----------------+-------------+--------------------------------------+
| query       | String   | -               | ""          | Pass mango query as input.           |
|             |          |                 |             | Query should be without 'selector'   |
|             |          |                 |             | keyword.                             |
+-------------+----------+-----------------+-------------+--------------------------------------+
| apikey      | String   | -               | ""          | Not yet implemented                  |
+-------------+----------+-----------------+-------------+--------------------------------------+
| documentType| String   | record          | "record"    | Documents of CouchDB are categorized |
|             |          | record_schema   |             | in four types depending on the type  |
|             |          | internal        |             | of data document contains.           |
|             |          | internal_schema |             | 'record' doc type contains user data.|
|             |          |                 |             | 'record_schema' doc type contains    |
|             |          |                 |             | schema of document "record".         |
|             |          |                 |             | 'internal' doc type contains data of |
|             |          |                 |             | internal structures of couchDb.      |
|             |          |                 |             | 'internal_schema' doc type contains  |
|             |          |                 |             | schema of document "internal".       |
+-------------+----------+-----------------+-------------+--------------------------------------+
| filterMode  | String   | 0, 1, 2, 3      | "3"         | The request will be processed based  | 
|             |          |                 |             | on the filterMode.                   |
|             |          |                 |             | 0 - Local Mode                       | 
|             |          |                 |             | In local mode query will be processed|
|             |          |                 |             | locally i.e on the data which is     |
|             |          |                 |             | available in mobile storage.         |
|             |          |                 |             | 1 - API Mode                         |
|             |          |                 |             | In API Mode the request will be      |
|             |          |                 |             | forwarded to server.                 | 
|             |          |                 |             | 2 - First Local Mode then API Mode   |
|             |          |                 |             | First query will be processed locally|
|             |          |                 |             | if data is not found then request    |
|             |          |                 |             | will be forwarded to server.         |
|             |          |                 |             | 3 - First API Mode then Local Mode   | 
|             |          |                 |             | First query will be forwarded to     |
|             |          |                 |             | server if data is not found and if   |   
|             |          |                 |             | device is offline then query will    | 
|             |          |                 |             | be processed locally.                |
+-------------+----------+-----------------+-------------+--------------------------------------+
| isChannelDoc| Boolean  | true/false      | false       | To get channel documents i.e pluginid|
|             |          |                 |             | '0' set this flag as 'true' else     |
|             |          |                 |             | it will return documents of currently|
|             |          |                 |             | open microapp.                       |
+-------------+----------+-----------------+-------------+--------------------------------------+
| skip        | Integer  | 0 or value > 0  | 0           | To skip specific number of           |
|             |          |                 |             | documents while processing the query.|
+-------------+----------+-----------------+-------------+--------------------------------------+
| limit       | Integer  | 0 or value > 0  | 5           | To return only specific number of    |
|             |          |                 |             | documents in callback response       |
+-------------+----------+-----------------+-------------+--------------------------------------+

NOTE: The callback response data will be associated with key 'schema' if documentType parameter 
is record_schema or internal_schema.
It will be associated with key 'data' if documentType parameter is record or internal.
Else string response will be provided.   


.. code:: javascript

	DronaHQ.sync.getDocument(documentName, query, apikey, documentType,
							 filterMode, isChannelDoc, skip, limit);

	Sample:
	Without Query:
		DronaHQ.sync.getDocument("","","","","0",true,0,25);

	With Query:
		var mangoQuery = {channel_id: 108,"created_by":"neeraj@dronamobile.com"}
		DronaHQ.sync.getDocument("",JSON.stringify(mangoQuery),"","","0",true,0,25);













