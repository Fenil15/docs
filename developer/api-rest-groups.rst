Groups
========

DronaHQ Plugin REST API provide read/write access to DronaHQ group(s) through the following action(s)-

	- :ref:`put-users-to-grp`
	
.. _put-users-to-grp:

Put user(s) to group
----------------------

This API can be used to assign a list of users to a group and can also be used to removes a list of users assigned to a group within the |scope| of its **tokenkey**.

.. |scope| raw:: html

   <a href="api-documentation-plugin-rest-auth.html" target="_blank">scope</a>
   
**ENDPOINT**

.. code:: text
	
	PUT /groups/{groupId}/actions/assign_user

**REQUEST PARAMETERS**

**Url segment**

+-------------------+-------------+------------------------------------------------------------------------------------------------+
| Parameter         | Value Type  | Description                                                                                    |
+===================+=============+================================================================================================+
| groupId           | integer     | *Required.* The unique id of the group in concern                                              |
+-------------------+-------------+------------------------------------------------------------------------------------------------+

**Request body bata**

+-------------------+-------------+------------------------------------------------------------------------------------------------+
| Parameter         | Value Type  | Description                                                                                    |
+===================+=============+================================================================================================+
| token_key         | string      | *Required.* Your API key. Check |authentication| for more details                              |
+-------------------+-------------+------------------------------------------------------------------------------------------------+
| assign            | array       | *Optional.* An integer array of unique ids of users to be assigned to the group.               |
+-------------------+-------------+------------------------------------------------------------------------------------------------+
| remove            | array       | *Optional.* An integer array of unique ids of users to be removed from the group.              | 
+-------------------+-------------+------------------------------------------------------------------------------------------------+

.. |authentication| raw:: html

   <a href="api-rest-auth.html" target="_blank">authentication</a>
   
Please note that either assign or remove should contain at least one user id.

**RESPONSE FORMAT**

On error, the header status code is an error code and the response body contains an |error object|. On success, the HTTP status code in the response header is 200 OK and the response body contains a JSON object with a list of users successfully *assigned to or removed from* a group. 

.. |error object| raw:: html

   <a href="api-documentation-plugin-rest-response-status-code.html#error-response-body" target="_blank">error object</a>
   
Please note that even when the HTTP status code in the response header is 200 OK,  *assigning or removing* users from a group might fail for other reasons such as the user is not a valid user, or the API itself failed to perform the operation. In such cases, the response body would contain an array of the invalid users in the invalid_users field and an array of users for whom the API operation failed in the failed_users field.

**Success response data**

+-----------------+------------+-------------------------------------------------------------------------------------------------------------------------+
| Parameter       | Value Type | Description                                                                                                             |
+=================+============+=========================================================================================================================+
| group_id        | integer    | The unique id of the group in concern.                                                                                  |
+-----------------+------------+-------------------------------------------------------------------------------------------------------------------------+
| assigned_users  | array      | An integer array of unique user ids successfully assigned to the group.                                                 |
+-----------------+------------+-------------------------------------------------------------------------------------------------------------------------+
| removed_users   | array      | An integer array of unique user ids successfully removed from the group.                                                |
+-----------------+------------+-------------------------------------------------------------------------------------------------------------------------+
| invalid_users   | array      | An integer array of unique user ids that are not valid.                                                                 |
+-----------------+------------+-------------------------------------------------------------------------------------------------------------------------+
| failed_users    | array      | An integer array of unique id of users on which the operation of assigning/removing failed. Retry again with these      |
|                 |            | users, if problem persists contact our support desk.                                                                    |
+-----------------+------------+-------------------------------------------------------------------------------------------------------------------------+
