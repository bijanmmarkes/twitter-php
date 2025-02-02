[Twitter for PHP](https://phpfashion.com/twitter-for-php)  [![Buy me a coffee](https://files.nette.org/images/coffee1s.png)](https://nette.org/make-donation?to=twitter-php)
================================

[![Downloads this Month](https://img.shields.io/packagist/dm/dg/twitter-php.svg)](https://packagist.org/packages/dg/twitter-php)

Twitter for PHP is a very small and easy-to-use library for sending
messages to Twitter and receiving status updates.

It requires PHP 5.4 or newer with CURL extension and is licensed under the New BSD License.
You can obtain the latest version from our [GitHub repository](https://github.com/dg/twitter-php)
or install it via Composer:

	composer require dg/twitter-php


[Support Me](https://github.com/sponsors/dg)
--------------------------------------------

Do you like Nette DI? Are you looking forward to the new features?

[![Buy me a coffee](https://files.nette.org/icons/donation-3.svg)](https://github.com/sponsors/dg)

Thank you!


Usage
-----
Sign in to the https://twitter.com and register an application from the https://apps.twitter.com page. Remember
to never reveal your consumer secrets. Click on My Access Token link from the sidebar and retrieve your own access
token. Now you have consumer key, consumer secret, access token and access token secret.

Create object using application and request/access keys

```php
use DG\Twitter\Twitter;

$twitter = new Twitter($consumerKey, $consumerSecret, $accessToken, $accessTokenSecret);
```

The send() method updates your status. The message must be encoded in UTF-8:

```php
$twitter->send('I am fine today.');
```

The load() method returns the 20 most recent status updates
posted by you:

```php
$statuses = $twitter->load(Twitter::ME);
```

or posted by you and your friends:

```php
$statuses = $twitter->load(Twitter::ME_AND_FRIENDS);
```
or most recent mentions for you:

```php
$statuses = $twitter->load(Twitter::REPLIES);
```
Extracting the information from the channel is easy:

```php
foreach ($statuses as $status) {
	echo "message: ", Twitter::clickable($status);
	echo "posted at " , $status->created_at;
	echo "posted by " , $status->user->name;
}
```

The static method `Twitter::clickable()` makes links, mentions and hash tags in status clickable.

The authenticate() method tests if user credentials are valid:

```php
if (!$twitter->authenticate()) {
	die('Invalid name or password');
}
```

The search() method provides searching in twitter statuses:

```php
$results = $twitter->search('#nette');
```

The returned result is a again array of statuses.


Error handling
--------------

All methods throw a `DG\Twitter\Exception` on error:

```php
try {
	$statuses = $twitter->load(Twitter::ME);
} catch (DG\Twitter\Exception $e) {
	echo "Error: ", $e->getMessage();
}
```

Additional features
-------------------

The `authenticate()` method tests if user credentials are valid:

```php
if (!$twitter->authenticate()) {
	die('Invalid name or password');
}
```

The `getRequestToken()` method allows a Consumer application to obtain an OAuth Request Token to request user authorization:
 - Documentation/Use-Cases: https://developer.twitter.com/en/docs/authentication/api-reference/request_token
 - Define a Callback URL in your Twitter Application (Required): https://developer.twitter.com/en/docs/apps/callback-urls
```php
# You can Initialize a new Twitter object using only the Consumer Key and Secret
$this->twitter = new Twitter($consumerKey, $consumerSecret);
# Call the getRequestToken() using the callback URL defined in your twitter application.
$response = $this->twitter->getRequestToken('https://localhost.com/twitter-callback-url');
```

Example Response:

```php 
{
    "oauth_token": "x-oFdAAAAAABVLnXXXXXXXX_XXX",
    "oauth_token_secret": "A810fifujUZXXXXXXXXXXXXXXXXXXXXX",
    "oauth_callback_confirmed": "true"
}
```


Other commands
--------------

You can use all commands defined by [Twitter API 1.1](https://dev.twitter.com/rest/public).
For example [GET statuses/retweets_of_me](https://dev.twitter.com/rest/reference/get/statuses/retweets_of_me)
returns the array of most recent tweets authored by the authenticating user:

```php
$statuses = $twitter->request('statuses/retweets_of_me', 'GET', ['count' => 20]);
```

Changelog
---------
v4.1 (11/2019)
- added Delete Method (#68)
- token is optional throughout + supply get() method

v4.0 (2/2019)
- requires PHP 7.1 and uses its advantages like typehints, strict types etc.
- class Twitter is now DG\Twitter\Twitter
- class TwitterException is now DG\Twitter\Exception

v3.8 (2/2019)
- Twitter::sendDirectMessage() uses new API
- Twitter::clickable: added support for $status->full_text (#60)

v3.7 (3/2018)
- minimal required PHP version changed to 5.4
- Twitter::send() added $options
- Twitter::clickable() now works only with statuses and entites
- fixed coding style

v3.6 (8/2016)
- added loadUserFollowersList() and sendDirectMessage()
- Twitter::send() allows to upload multiple images
- changed http:// to https://

v3.5 (12/2014)
- allows to send message starting with @ and upload file at the same time in PHP >= 5.5

v3.4 (11/2014)
- cache expiration can be specified as string
- fixed some bugs

v3.3 (3/2014)
- Twitter::send($status, $image) can upload image
- added Twitter::follow()

v3.2 (1/2014)
- Twitter API uses SSL OAuth
- Twitter::clickable() supports media
- added Twitter::loadUserInfoById() and loadUserFollowers()
- fixed Twitter::destroy()

v3.1 (3/2013)
- Twitter::load() - added third argument $data
- Twitter::clickable() uses entities; pass as parameter status object, not just text
- added Twitter::$httpOptions for custom cURL configuration

v3.0 (12/2012)
- updated to Twitter API 1.1. Some stuff deprecated by Twitter was removed:
	- removed RSS, ATOM and XML support
	- removed Twitter::ALL
	- Twitter::load() - removed third argument $page
	- Twitter::search() requires authentication and returns different structure
- removed shortening URL using http://is.gd
- changed order of Twitter::request() arguments to $resource, $method, $data

v2.0 (8/2012)
- added support for OAuth authentication protocol
- added Twitter::clickable() which makes links, @usernames and #hashtags clickable
- installable via `composer require dg/twitter-php`

v1.0 (7/2008)
- initial release


-----
(c) David Grudl, 2008, 2016 (https://davidgrudl.com)
