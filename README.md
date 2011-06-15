
RestfulConsumer
===============

Heavily inspired by John Nunemaker's [HTTParty](https://github.com/jnunemaker/httparty), RestfulConsumer tries to make API consumption as simple as possible--at least on the Java side of the world.

Usage
-----

### Twitter

	public class Twitter extends RestfulConsumer {
	
	  public Twitter() {
		  this.setBaseURI("http://api.twitter.com/1");
		  this.setFormat("json");
	  }
	
	  public JSONArray userTimeline(String screen_name) throws JSONException, Exception {
		  ArrayList<NameValuePair> options = new ArrayList<NameValuePair>();
		  options.add(new BasicNameValuePair("screen_name", screen_name));
		  return new JSONArray(this.get("/statuses/user_timeline", options));
	  }
	
	  public JSONObject showUser(String screen_name) throws JSONException, Exception {
		  ArrayList<NameValuePair> options = new ArrayList<NameValuePair>();
		  options.add(new BasicNameValuePair("screen_name", screen_name));
		  return new JSONObject(this.get("/users/show", options));		
	  }
	
	}
	
	/* Twitter client = new Twitter();
	   JSONArray tweets = client.userTimeline("adelevie");
	   JSONObject user = client.showUser("adelevie");
	*/

### POSTing to a Rails back-end

Create a simple Rails app

	rails new SomeApp
	cd SomeApp
	rails g scaffold Widget name:string
	rake db:migrate
	rails server

In `app/controllers/application_controller.rb`, remove `protect_from_forgery`.

Use localtunnel (do this in a new tab, still cd'd to SomeApp)

	gem install localtunnel
	localtunnel 3000

Java code

	public class PostExample extends RestfulConsumer {
	
	  public PostExample() {
		  setBaseURI("http://____.localtunnel.com"); //localtunnel URL goes here
		  setFormat("xml");
	  }
	
	  public String showWidget(String widgetID) throws Exception {
		  ArrayList<NameValuePair> options = new ArrayList<NameValuePair>();
		  return this.get("/widgets/" + widgetID, options);
	  }
	
	  public String createWidget(String widgetName) throws Exception {
		  ArrayList<NameValuePair> options = new ArrayList<NameValuePair>();
		  options.add(new BasicNameValuePair("widget[name]", widgetName));
		  return this.post("/widgets", options);
	  }
	}
	 /* PostExample client = new PostExample();
	    String response = client.createWidget("Foo");
	 */

Notice that by following the `new BasicNameValuePair(model_name[attribute_name], attribute_value)` convention, you don't have to change a single line in your Rails controllers. `ModelName.new(params[:model_name])` just works.

### HTTP Basic Auth
Warning: HTTP Basic Auth is only as secure as its transport mechanism. HTTP, which is vulnerable to traffic sniffers. I found this [blog post](http://www.skorks.com/2009/08/is-basic-authentication-really-insecure/) to be particularly useful in evaluating risk.

	public class BasicAuthExample extends RestfulConsumer {
	  public BasicAuthExample() {
		  setBaseURI("http://____.localtunnel.com"); // localtunnel url goes here
		  setFormat("xml");
		  setAuthHash("a Base64-encoded hash goes here. schema is username:password");
	  }
	
	  public String showWidget(String widgetID) throws Exception {
		  ArrayList<NameValuePair> options = new ArrayList<NameValuePair>();
		  return this.get("/widgets/" + widgetID, options);
	  }
	}

I added [Devise](https://github.com/plataformatec/devise) to my sample Rails app and then followed [this](https://github.com/plataformatec/devise/wiki/How-To:-Use-HTTP-Authentication) tutorial for adding HTTP Basic Authentication.

Because the Android SDK does not include a Base64 encoder, I decided to encode my username:password hash outside of the app, and then just paste it into the source.



Limitations
-----------

Currently, only GET and POST requests are supported.

### Thanks
* [John Nunemaker](http://railstips.org/about/), for creating HTTParty
* [Luke Lowrey](http://lukencode.com/about), whose [RestClient](http://lukencode.com/2010/04/27/calling-web-services-in-android-using-httpclient/) class I borrowed many snippets from

License
-------

Copyright (c) 2011 Alan deLevie

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

