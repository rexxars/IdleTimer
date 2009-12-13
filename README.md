IdleTimer
=========

Determines when the user is idle (not interacting with the page) so that you can respond appropriately.

![Screenshot](http://rexxars.com/project/idletimer/title.png)

How to use
----------

An easy setup of IdleTimer might be something like this:

	#JS
	var myIdleTimer = new IdleTimer(window, { timeout: 60000 });
	myIdleTimer.addEvent('idle', function() {
		alert('You have been idle for 60 seconds!');
	});

This would launch an alert box after the user has not moved his mouse button over the document, clicked any mouse/keyboard buttons or scrolled using the mouse scroll wheel.

If you want to use the default settings (60 second timeout), you can also do it even easier by doing something like this:

	#JS
	window.addEvents({
		'idle': function() { alert('60 seconds have passed!'); },
		'active': function() { alert('You just became active again!'); }
	});

If you want to apply the IdleTimer to a specific element on the page, you can get an IdleTimer instance and set it's options like this:

	#JS
	$('myDiv').get('idle'); // Returns an IdleTimer instance
	$('myDiv').set('idle', { timeout: 5000, events: ['mouseover', 'mousedown'] }); // Creates an IdleTimer instance with the given options (does not start it)

Syntax
------

	#JS
	new IdleTimer(element, [options]);

Arguments
---------

	1. element - (mixed) An Element or an id string.
	2. options - (object, optional) the options described below:

Options
-------

* timeout : (number) Number of idle milliseconds before firing the idle event. Defaults to 60000 (one minute).
* events  : (array) Event names that trigger the active event (resets the idle time). Defaults to ['mousemove', 'keydown', 'mousewheel', 'mousedown']

Events
------

* idle           : Fired when the user has reached the set amount of idle milliseconds.
* active         : Fired when the user becomes active.
* start          : Fired when the events have been added and the timer is starting for the first time.
* stop           : Fired when the timer has been stopped and the events have been removed.
* timeoutChanged : Fired when the threshold for idle milliseconds has been set to a new value. Gets passed the new value.

