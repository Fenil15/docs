.. _ref-device-app:

App
===

The App API provides an easy way interact with a particular instance of a micro-app.

It has the following method(s) -

	- :ref:`navigate() <navigate>`
	- :ref:`exitapp() <exit-app>`
	- :ref:`openAppBugFeedback() <open-app-bug-feedback>`
	- :ref:`getAppBugFeedback() <getAppBugFeedback>`
	- :ref:`submitBugFeedbackData() <submitBugFeedbackData>`
	- :ref:`setStatusBar() <set-status-bar>`
	- :ref:`setBottomBar() <set-bottom-bar>`
	- :ref:`resetiPhoneXColor() <reset-iphonex-color>`
	- :ref:`resetStatusBarAndBottomBar() <reset-status-bar-and-bottom-bar>`

.. _navigate:

navigate()
----------

Using this method you can open any kind of screen of the app using mentioned keys.
Destination (dest) is the screen to be opened for user. 
Destination have few sub type(dest_type) as suppported for by them.
Destination screen are mostly defined by unique ID of the objects passed in dest_id 


**REQUEST PARAMETERS**

+--------------+----------+-----------------------------------------+
|Parameter     |Type      |Description                              |
+==============+==========+=========================================+
|dest          |string    |Required. destination where user will be |
|              |          |navigated. Possible values are mentinoed |
+--------------+----------+-----------------------------------------+
|dest_type     |string    |Optional. Sub type of screen to be open  |
|              |          |Possible values are mentioned below      |
+--------------+----------+-----------------------------------------+
|dest_id       |string    |Optional. ID for which screen will open  |
+--------------+----------+-----------------------------------------+
|folder_id     |string    |Optional. Folder id in which icon belongs|
+--------------+----------+-----------------------------------------+
|parameters    |object    |Optional. Data to be passed to next app  |
+--------------+----------+-----------------------------------------+
|fnsuccess     |function  |*Optional* Callback function it will be  |
|              |          |called on success                        |
+--------------+----------+-----------------------------------------+
|fnerror       |function  |*Optional* Callback function it will be  |
|              |          |called on failure                        |
+--------------+----------+-----------------------------------------+

**SUPPORTED PARAMETERS**

+--------------+-------------------+-------------------------------------+
|Destination   |Destination Type   |Description                          |
+==============+===================+=====================================+
|Icon          | action, category  |Opens icon for thre provided microapp|
+--------------+-------------------+-------------------------------------+
|              |                   |or content category of provided ID   |
+--------------+-------------------+-------------------------------------+
|CONTENT       | v, audio, pdf,  n,|Opens video, audio, PDF, article,    |
+--------------+-------------------+-------------------------------------+
|              | embed, article, p,|embed, URL, Slide/PPT, survey, quiz, |
+--------------+-------------------+-------------------------------------+
|              | s, q, form, ig,   |form, image gallery and text content |
+--------------+-------------------+-------------------------------------+
|              | text              |respectively for provided ID         |
+--------------+-------------------+-------------------------------------+
|logout        |                   |Logout the user from the app         |
+--------------+-------------------+-------------------------------------+
|setting       |                   |Opens setting screen of user in app  |
+--------------+-------------------+-------------------------------------+
|about         |                   |Opens the about screen of the app    |
+--------------+-------------------+-------------------------------------+
|support       |                   |Opens the support screen of the app  |
+--------------+-------------------+-------------------------------------+
|INBOX         |                   |Opens the inbox screen for the user  |
+--------------+-------------------+-------------------------------------+
|search        | private           |Opens the icon search screen of app  |
+--------------+-------------------+-------------------------------------+
|favourite     | private           |Opens icon favourite screen of app   |
+--------------+-------------------+-------------------------------------+
|downloads     | private           |Opens the download screen for user   |
+--------------+-------------------+-------------------------------------+
|bookmarks     | private           |Opens the bookmark screen for user   |
+--------------+-------------------+-------------------------------------+

.. code:: javascript

	var obj = {};
	obj.dest = 'icon';
	obj.dest_type = 'category';
	obj.dest_id = $(this).data("appid");
	obj.folder_id = '0';
	obj.parameters = {};
	DronaHQ.app.navigate(obj);

.. _exit-app:

exitapp()
---------

Using this method allows you to exit the current instance of the micro-app.

.. code:: javascript

	DronaHQ.app.exitApp();

.. _open-app-bug-feedback:

openAppBugFeedback()
--------------------

This will open Bug/Feedback screen in the App.

+----------------+----------+-----------------------------------------+
|Parameter       |Type      |Description                              |
+================+==========+=========================================+
|type            |string    |Type of Bug/Feedback dropdown preselected|
|                |          |. Possible values : "feedback" , "bug".  |
|                |          |"bug" for Bug dropdown preselected,      |
|                |          |"feedback" for Feedback dropdown         |
|                |          |preselected                              |
+----------------+----------+-----------------------------------------+
|title           |string    |Prefilled title to be set on opening the |
|                |          |Bug/Feedback Screen                      |
+----------------+----------+-----------------------------------------+
|description     |string    |Prefilled description to be set on       |
|                |          |opening the Bug/Feedback Screen          |
+----------------+----------+-----------------------------------------+

.. code:: javascript

	var fnSuccess = function(){
		console.log('Opened bug/feedback screen successfully');
	};

	var fnError = function(err){
		console.error('Failed to open bug/feedback screen. Error: ' + err);
	};

	var type = "feedback"; // can be either feedback/bug
	var title = "Prefilled Title";
	var description = "Prefilled Description";

	DronaHQ.app.openAppBugFeedback(fnSuccess, fnError, type, title, description);

.. _getAppBugFeedback:

getAppBugFeedback()
-------------------

This method used get the details of bug feedback given by user, the following Parameters are received: title, type of bug and description.

.. code:: javascript

    var fnsuccess = function(data){
        console.log(data)
    }
	var fnError = function(err) {
        console.log(err);
    }
    DronaHQ.app.getAppBugFeedback();

.. _submitBugFeedbackData:

submitBugFeedbackData()
-----------------------

This method used to send the bug or feedback data.

**REQUEST PARAMETERS**

+----------------+----------+-----------------------------------------+
|Parameter       |Type      |Description                              |
+================+==========+=========================================+
|type            |string    |Type of Bug/Feedback dropdown preselected|
|                |          |. Possible values : "feedback" , "bug".  |
|                |          |"bug" for Bug dropdown preselected,      |
|                |          |"feedback" for Feedback dropdown         |
|                |          |preselected                              |
+----------------+----------+-----------------------------------------+
|title           |string    |Prefilled title to be set on opening the |
|                |          |Bug/Feedback Screen                      |
+----------------+----------+-----------------------------------------+
|description     |string    |Prefilled description to be set on       |
|                |          |opening the Bug/Feedback Screen          |
+----------------+----------+-----------------------------------------+
|image           |string    |screenshot of current screen is send in  |
|                |          |Base64 format                            |
+----------------+----------+-----------------------------------------+
|source_type     |string    |type of source from which bug is raised  |
+----------------+----------+-----------------------------------------+
|icon_id         |integer   |action button ID                         |
+----------------+----------+-----------------------------------------+
|plugin_evn      |string    |Type of environment (Prod,Dev,Beta)      |
+----------------+----------+-----------------------------------------+
|plugin_version  |string    |current version of plugin (Microapp)     |
+----------------+----------+-----------------------------------------+

.. code:: javascript

    var fnOnSuccess = function (data) {
        console.log(data)
    };

    var fnOnError = function (err) {
        console.log(err)
    };
	
    DronaHQ.app.submitBugFeedbackData(fnOnSuccess, fnOnError, type, title, description, image, source_type, icon_id, plugin_evn, plugin_version);

.. _set-status-bar:

setStatusBar()
--------------

Used to customize status bar settings.

+----------------+------------+-----------------------------------------+
|Parameter       |Type        |Description                              |
+================+============+=========================================+
|statusBarObject |JSONObject  |Object contains below mentioned keys     |
+----------------+------------+-----------------------------------------+

+----------------+----------+--------------+-----------------------------------------+
|Parameter       |Type      |Default Value |Description                              |
+================+==========+==============+=========================================+
|isHidden        |String    |false         |To hide or show status bar.              |
|                |          |              |Set this flag true to hide status bar.   |
+----------------+----------+--------------+-----------------------------------------+
|color1          |String    |"d5d5d5"      |The color of status bar will set to the  |
|                |          |              |color code provided as input             |
|                |          |              |Note: Color code (6 Digit code) should   |
|                |          |              |be without '#'                           |
+----------------+----------+--------------+-----------------------------------------+
|color2          |String    |""            |In order to set gradient color on status |
|                |          |              |bar provide color code as input.         |
|                |          |              |If empty then solid color will be set    |
|                |          |              |(color1) on status bar.                  |
|                |          |              |Note: Color code (6 Digit code) should   |
|                |          |              |be without '#'.                          |
+----------------+----------+--------------+-----------------------------------------+
|isTextThemeDark |String    |true          |Used to set text color on statusbar.     |
|                |          |              |By default it is black. Set it as false  |
|                |          |              |to set text color as white.              |
|                |          |              |Eg. TextColor of time on status bar      |
+----------------+----------+--------------+-----------------------------------------+

NOTE: 1.Minimum android OS required to set status bar color is Lollipop. 2.Minimum android OS required to set isTextThemeDark is Marshmallow.

.. code:: javascript

	var statusBarObject = {"isHidden":"false", "color1":"d1d1d1", "color2":"", "isTextThemeDark":"true"}

	DronaHQ.app.setStatusBar(fnSuccess, fnError, statusBarObject);

.. _set-bottom-bar:

setBottomBar()
--------------

Used to customize bottom bar settings.

+----------------+------------+-----------------------------------------+
|Parameter       |Type        |Description                              |
+================+============+=========================================+
|bottomBarObject |JSONObject  |Object contains below mentioned keys     |
+----------------+------------+-----------------------------------------+

+----------------+----------+--------------+-----------------------------------------+
|Parameter       |Type      |Default Value |Description                              |
+================+==========+==============+=========================================+
|isHidden        |String    |false         |To hide or show bottom bar.              |
|                |          |              |Set this flag true to hide status bar.   |
+----------------+----------+--------------+-----------------------------------------+
|color1          |String    |""            |The color of bottom bar will set to the  |
|                |          |              |color code provided as input             |
|                |          |              |If empty mobile device will set default  |
|                |          |              |color                                    |
|                |          |              |Note: Color code (6 Digit code) should   |
|                |          |              |be without '#'                           |
+----------------+----------+--------------+-----------------------------------------+
|color2          |String    |""            |In order to set gradient color on bottom |
|                |          |              |bar provide color code as input.         |
|                |          |              |If empty then solid color will be set    |
|                |          |              |(color1) on status bar.                  |
|                |          |              |Note: Color code (6 Digit code) should   |
|                |          |              |be without '#'.                          |
+----------------+----------+--------------+-----------------------------------------+

NOTE: Minimum android OS required to set status bar color is Lollipop.

.. code:: javascript

	var bottombarObject = {"isHidden":"false", "color1":"d5d5d5", "color2":""}

	DronaHQ.app.setBottomBar(fnSuccess, fnError, bottombarObject);


.. _reset-iphonex-color:

resetiPhoneXColor()
-------------------

.. note::

	This method is deprecated now refer to resetStatusBarAndBottomBar

This method is used to reset the top (status bar color) and bottom (navigation bar color) in android as well as iphone

.. code:: javascript

	DronaHQ.app.resetiPhoneXColor(fnsuccess, fnerror);
	

.. _reset-status-bar-and-bottom-bar:

resetStatusBarAndBottomBar()
----------------------------

Use this method to reset the settings of status bar and bottom bar back to default value.

.. code:: javascript

	DronaHQ.app.resetStatusBarAndBottomBar();


