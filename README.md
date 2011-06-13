
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


Limitations
-----------

RestfulConsumer is brand new, and can be rough around the edges. Currently, only GET and POST requests are supported.

### Thanks
* [John Nunemaker](http://railstips.org/about/), for creating HTTParty
* [Luke Lowrey](http://lukencode.com/about), whose [RestClient](http://lukencode.com/2010/04/27/calling-web-services-in-android-using-httpclient/) class I borrowed many snippets from

