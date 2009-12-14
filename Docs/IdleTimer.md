Class: IdleTimer {#IdleTimer}
=============================

Determines when the user is idle (not interacting with the page) so that you can respond appropriately.

### Implements:

Options, Events

### Notes:

- Demo can be found here: [http://rexxars.com/project/idletimer/](http://rexxars.com/project/idletimer/) 

### Syntax:

	var myIdleTimer = new IdleTimer(element[, options]);

### Arguments:

1. element - (*mixed*) Either an Element or an id reference to the element you want to monitor. Also accepts window/document.
2. options - (*object*, optional) See below.

### Options:

* timeout - (*number*: defaults to 60000 - one minute) Number of idle milliseconds before firing the idle event. Defaults to 60000 (one minute).
* events  - (*array*: defaults to ['mousemove', 'keydown', 'mousewheel', 'mousedown']) Event names that trigger the active event (resets the idle time).

### Events:

#### idle

Fired when the user has reached the set amount of idle milliseconds.

##### Signature:

	onIdle()
	
#### active

Fired when the user becomes active (and the previous state was idle).

##### Signature:

	onActive()

#### start

Fired when the events have been added and the timer is starting for the first time.

##### Signature:

	onStart()
	
#### stop

Fired when the timer has been stopped and the events have been removed.

##### Signature:

	onStop()
	
#### timeoutChanged

Fired when the threshold for idle milliseconds has been set to a new value.

##### Signature:

	onTimeoutChanged(newTimeout)
	
##### Arguments:

1. newTimeout - (*number*) The new timeout value, in milliseconds.

### Properties:

* isIdle  - (*boolean*) True if the user is idle.
* started - (*boolean*) True if the start() method has been called and the element is receiving events.

### Returns:

* (*object*) A new IdleTimer instance.

### Example:

	var myIdleTimer = new IdleTimer(window, {timeout: 30000});
	myIdleTimer.start();

IdleTimer Method: start {#IdleTimer:start}
------------------------------------------

Adds the events defined in the options to the given element and starts the idle timer.

### Syntax:

	myIdleTimer.start();

### Returns:

* (*object*) This IdleTimer instance.

### Example:

	var myIdleTimer = new IdleTimer(window, {timeout: 30000, events: ['mousemove', 'keydown']});
	myIdleTimer.start();

IdleTimer Method: stop {#IdleTimer:stop}
----------------------------------------

Removes the events defined in the options to the given element and stops the idle timer.

### Syntax:

	myIdleTimer.stop();

### Returns:

* (*object*) This IdleTimer instance.

### Example:

	var myIdleTimer = new IdleTimer(window, {timeout: 30000, events: ['mousemove', 'keydown']});
	myIdleTimer.start();
	
	// Only listen for the first 30 seconds
	myIdleTimer.stop.delay(30000);

IdleTimer Method: getIdleTime {#IdleTimer:getIdleTime}
------------------------------------------------------

Calculates how much time has passed since the user was last active.

### Syntax:

	myIdleTimer.getIdleTime([seconds]);

### Arguments:

1. returnSeconds - (*boolean*, optional) Will return seconds if set to true.

### Returns:

* (*number*) The number of milliseconds/seconds has passed since the user was last active.

### Example:

	var myIdleTimer = new IdleTimer(window).start();
	setTimeout(function() {
		alert(myIdleTimer.getIdleTime()); // Will alert around 4000 if no activity is recorded before that
	}, 4000);

IdleTimer Method: setTimeout {#IdleTimer:setTimeout}
----------------------------------------------------

Sets the number of milliseconds is concidered "idle".

### Syntax:

	myIdleTimer.setTimeout(newTime, whenActive);

### Arguments:

1. newTime - (*number*) The number of idle milliseconds to wait before firing the idle event.
2. whenActive - (*boolean*, optional) Pass true to avoid overriding any ongoing timer.

### Returns:

* (*object*) This IdleTimer instance.

### Example:

	var myIdleTimer = new IdleTimer(window).start();
	setTimeout(function() {
		myIdleTimer.setTimeout(10000));
		/* Will set the idle timeout value to 10 seconds, which would trigger
		   the idle event to be fired if the user has been idle for ten seconds. */
	}, 30000);

### Notes:

The method will also attempt to fix any difference in the old and new timeout values, unless you pass true as whenActive - in this case the new timeout will be in play the next time the user is active again.

IdleTimer Method: bind {#IdleTimer:bind}
----------------------------------------

(*Private*) Binds the events to the element. Used internally and should not be called. Use [IdleTimer:start][] instead.

### Syntax:

	myIdleTimer.bind();

IdleTimer Method: idle {#IdleTimer:idle}
----------------------------------------

Sets the state of this instance to idle and fires the idle event. This is usually only called internally, but you could also call it manually.

### Syntax:

	myIdleTimer.idle();

IdleTimer Method: active {#IdleTimer:active}
--------------------------------------------

Sets the state of this instance to active and fires the active event if the previous state was idle. This is usually only called internally, but you could also call it manually. This could be useful for flash overlays and similar where mouse events do not trigger to the underlying document.

### Syntax:

	myIdleTimer.active();

### Example:	

	var myIdleTimer = new IdleTimer(window).start();
	myMoviePlayer.addEventListener('mousemove', myIdleTimer.active);
	
Element Property: idle {#Element-Properties:idle}
-------------------------------------------------

### Setter

Sets a default IdleTimer instance for an Element. Useful to easily retrieve an associated IdleTimer.

#### Syntax:

	el.set('idle'[, options]);

#### Arguments:

1. options - (*object*) The IdleTimer options.

#### Returns:

* (*element*) The original element.

#### Example:

	myDiv.set('idle', {timeout: 5000, events: ['mousemove']});
	myDiv.addEvent('idle', function() {
		alert('You have not moved your mouse over myDiv for 5 seconds!');
	});

### Getter

Returns the previously set IdleTimer instance (or a new one with default options).

#### Syntax:

	el.get('idle'[, options]);

#### Arguments:

1. options - (*object*, optional) The IdleTimer options.  If passed, this method will generate a new instance of the IdleTimer class.

### Returns:

* (*object*) The IdleTimer instance.

#### Example:

	var timer = el.get('send', {timeout: 500}).start(); // Creates an IdleTimer instance, starts it and sets it to a local variable.
	timer.getIdleTime(); // Returns how many milliseconds have passed since the user was last active
	
Element Event: idle
-------------------

### Event: idle

Fired when the user has reached the set amount of idle milliseconds. Also available on window and document.

#### Examples:

	$('myElement').addEvent('idle', function() {
		alert('myElement now conciders you idle.');
	});

Element Event: active
---------------------

### Event: active

Fired when the user becomes active (when the previous state was idle)

#### Examples:

	$(window).addEvent('active', function() {
		alert('Welcome back!');
	});
	
