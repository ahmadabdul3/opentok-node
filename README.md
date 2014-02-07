# OpenTokSDK for Node.js

The OpenTok server SDKs include code for your web server. Use these SDKs to generate
[sessions](http://tokbox.com/opentok/tutorials/create-session/) and to obtain
[tokens](http://tokbox.com/opentok/tutorials/create-token/) for [OpenTok](http://www.tokbox.com/)
applications.

This version of the SDK also includes support for working with OpenTok 2.0 archives.

## Installation

To install using npm, add OpenTok to `package.json` and run `npm install`:

<pre>
"dependencies" : {  
    "opentok" : "2.0.0-beta",
    ...
} 
</pre>

To install as a regular npm package just type `npm install opentok`.

## Requirements

You need an OpenTok API key and API secret, which you can obtain at <https://dashboard.tokbox.com>.

Add the following code to the top of any file using the `opentok` module:

    var OpenTok = require('opentok');


## OpenTokSDK

In order to use any of the server side functions, you must first create an `OpenTokSDK` object with your developer credentials.  

You must pass in your OpenTok API key and *API secret (see <https://dashboard.tokbox.com>):

<pre>
var key = '';    // Replace with your API key  
var secret = '';  // Replace with your API secret  
var opentok = new OpenTok.OpenTokSDK(key, secret);
</pre>


## Creating Sessions

Use your `OpenTokSDK` object to create a `session_id`. See <http://tokbox.com/opentok/tutorials/create-session/> for more details.
`createSession` takes 2-3 parameters:  

* location [string] -- An IP address that the OpenTok servers will use to situate the session in its
global network. If you do not pass in a location hint, the OpenTok servers will be based on first
client connecting to the session.

* properties [object] -- Optional. Set p2p.preference as "enabled" to create a peer-to-peer
session, or to "disabled" to create an OpenTok server-enabled session. The OpenTok media server provides benefits not available in peer-to-peer sessions. For example, the OpenTok media server can
decrease bandwidth usage in multiparty sessions. Also, the OpenTok server can improve the quality of the user experience through [dynamic traffic shaping](http://www.tokbox.com/blog/quality-of-experience-and-traffic-shaping-the-next-step-with-mantis). For information on pricing, see the <a href="http://www.tokbox.com/pricing">OpenTok pricing page.

* callback [function(sessionId)] -- This is a function that handles the server response after session has been created. The result parameter of the function is the OpenTok session ID (a string).


### Example: peer-to-peer disabled (default)

<pre>
var location = '74.125.239.103'; // Use an IP address representative of the clients in your session.
var sessionId;
opentok.createSession(location, function(result){
  sessionId = result;
  // Do things with sessionId
});
</pre>

### Example: peer-to-peer enabled

<pre>
var location = '74.125.239.103'; // Use an IP address representative of the clients in your session.
var sessionId;
opentok.createSession(location, {'p2p.preference':'enabled'}, function(result){
  sessionId = result;
});
</pre>


## Generating Tokens

With the generated session ID and an OpenTokSDK object, you can start generating tokens for each
user. See <http://tokbox.com/opentok/tutorials/create-token/> for more details.

The `generateToken()` method takes in an object with 1-4 properties, and returns a token string:

* session_id [string] - REQUIRED. This token is tied to the session.
* role [string] - Optional. OpenTok.RoleConstants.{SUBSCRIBER|PUBLISHER|MODERATOR}. Publisher role used when omitted.
* expire_time [int] - Optional. Time when token will expire in unix timestamp.
* connection_data [string] - Optional. Stores static metadata to pass to other users connected to the session (e.g. names, user id, etc.).

<pre>
var token = opentok.generateToken({session_id:session_id, role:OpenTok.RoleConstants.PUBLISHER, connection_data:"userId:42"});
</pre>

## Examples

  Check out the basic working example in examples/app.js

### To run test suite:
    jasmine-node --coffee spec/


