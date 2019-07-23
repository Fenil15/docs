.. _ref-device-user:

User
====
This family of device APIs provided by dronahq.js helps in interacting with the user currently signed-in to Container App.

It has the following method(s) -

	- :ref:`getprofile() <get-profile>`
	- :ref:`setLoginDetails() <set-login-details>`


.. _get-profile:

getprofile()
------------

.. code:: javascript

	DronaHQ.user.getProfile(fnSuccess, fnError);
	
This method gives the profile object of the user logged into the app.

**USER PROFILE OBJECT -**

+--------------+----------+-----------------------------------------+
|Parameter     |Type      |Description                              |
+==============+==========+=========================================+
|uid	       |integer   |user id of the user currently signed-in  |
|              |          |to the container app                     |
+--------------+----------+-----------------------------------------+
|name	       |string    |Full name of the user currently signed-in|
|              |          |to the container app                     |
+--------------+----------+-----------------------------------------+
|email	       |object	  |Email id of the user currently signed-in |
|              |          |to the container app                     |
+--------------+----------+-----------------------------------------+
|designation   |string    |Designation of the user currently        |
|              |          |signed-in to the container app           |
+--------------+----------+-----------------------------------------+
|profile_image |object	  |Url of the profile image of the user     |
|              |          |currently signed-in to the container app |
+--------------+----------+-----------------------------------------+
|nonce	       |function  |A randrom string                         |
+--------------+----------+-----------------------------------------+

.. code:: javascript

	var fnSuccess = function(uData){
		console.log('User ID: ' + uData.uid);
		console.log('User Name: ' + uData.name);
		console.log('User Email: ' + uData.email);
		console.log('User Designation: ' + uData.designation);
		console.log('User Profile Image: ' + uData.profile_image);
		console.log('User Nonce: ' + uData.nonce);
	};

	var fnError = function(err){
		console.error('Failed to load user info. Error: ' + err);
	};

	DronaHQ.user.getProfile(fnSuccess, fnError);


.. _set-login-details:

setLoginDetails()
-----------------

This method is an alternative of login, when SSO sign-in process used into the app for user. 
In this sign-in process a cookie is set which needs to be sent to sever for authentication is ecypted using base64 encoding and passed using this method to server.


.. code:: javascript

	var cookie = 'base64 encrypted cookie'

	DronaHQ.user.getProfile(fnSuccess, fnError, cookie);