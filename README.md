angulartics-for-mixpanel
=========================

Vendor-agnostic analytics for AngularJS applications. 

# Install

## Bower
```sh
$ bower install angulartics-for-mixpanel
```

# Full path tracking (for pages without a router)
Introduced in 0.15.19 - support websites that do not use Angular `routes` or `states` on every page and still want to track full paths.  The modifications lead to the following behavior:

 - **Viewing page `http://host.com/routes#/route` will be tracked as `/routes#/route`.** The original version would only track the page as `/route`
 - **Viewing page `http://host.com/noroutes` will be tracked as `/noroutes`.**  This is useful for pages that do not contain Angular code besides initializing the base module.
 - **Viewing page `http://host.com/routes2` that loads a default route and changes the path to `http://host.com/routes2#/` will be tracked as `/routes2#/`.** This will only fire one pageview, whereas earlier versions would have fired two.

To enable this behavior, add the following to your configuration:

		...
		var yourApp = angular.module('YourApp', ['angulartics', 'angulartics.google.analytics'])
		    .config(function ($analyticsProvider) {
		        $analyticsProvider.firstPageview(true); /* Records pages that don't use $state or $route */
		        $analyticsProvider.withAutoBase(true);  /* Records full path */
		});

You can also use `$analyticsProvider.withBase(true)` instead of `$analyticsProvider.withAutoBase(true)` if you are using a `<base>` HTML tag.

# Setup


## for other providers

[Browse the website for detailed instructions.](http://luisfarzati.github.io/angulartics)


# Playing around

## Disabling virtual pageview tracking

If you want to keep pageview tracking for its traditional meaning (whole page visits only), set virtualPageviews to false:

	module.config(function ($analyticsProvider) {
		$analyticsProvider.virtualPageviews(false);     

## Programmatic tracking

Use the `$analytics` service to emit pageview and event tracking:

	module.controller('SampleCtrl', function($analytics) {
		// emit pageview beacon with path /my/url
	    $analytics.pageTrack('/my/url');

		// emit event track (without properties)
	    $analytics.eventTrack('eventName');

		// emit event track (with category and label properties for GA)
	    $analytics.eventTrack('eventName', { 
	      category: 'category', label: 'label'
        }); 

## Declarative tracking

Use `analytics-on` and `analytics-event` attributes for enabling event tracking on a specific HTML element:

	<a href="file.pdf" 
		analytics-on="click"
        analytics-if="myScope.shouldTrack"
		analytics-event="Download">Download</a>

`analytics-on` lets you specify the DOM event that triggers the event tracking; `analytics-event` is the event name to be sent.

`analytics-if` is a conditional check. If the attribute value evaluates to a falsey, the event will NOT be fired. Useful for user tracking opt-out, etc.

Additional properties (for example, category as required by GA) may be specified by adding `analytics-*` attributes:

	<a href="file.pdf" 
		analytics-on="click" 
		analytics-event="Download"
		analytics-category="Content Actions">Download</a>

or setting `analytics-properties`:

	<a href="file.pdf" 
		analytics-on="click" 
		analytics-event="Download"
		analytics-properties="{ category: 'Content Actions' }">Download</a>


## User tracking

You can assign user-related properties which will be sent along each page or event tracking thanks to:

    $analytics.setAlias(alias)
    $analytics.setUsername(username)
    $analytics.setUserProperties(properties)
    $analytics.setSuperProperties(properties)

Like `$analytics.pageTrack()` and `$analytics.eventTrack()`, the effect depends on the analytics provider (i.e. `$analytics.register*()`). Not all of them implement those methods.

The Google Analytics module lets you call `$analytics.setUsername(username)` or set up `$analyticsProvider.settings.ga.userId = 'username'`.


## Developer mode

You can disable tracking with:

    $analyticsProvider.developerMode(true);

You can also debug Angulartics by adding the following module:

    angular.module('myApp', [..., 'angulartics.debug'])

which will call `console.log('Page|Event tracking: ', ...)` accordingly.


# License

Angulartics is freely distributable under the terms of the MIT license.

Copyright (c) 2013 Luis Farzati

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

[![Bitdeli Badge](https://d2weczhvl823v0.cloudfront.net/luisfarzati/angulartics/trend.png)](https://bitdeli.com/free "Bitdeli Badge")

