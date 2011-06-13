
RestfulConsumer
===============

Heavily inspired by John Nunemaker's [HTTParty](https://github.com/jnunemaker/httparty), RestfulConsumer tries to make API consumption as simple as possible--at least on the Java side of the world.

Usage
-----

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

Limitations
-----------

RestfulConsumer is brand new, and can be rough around the edges. Currently, only GET and POST requests are supported. Nested URL paramaters will be supported when I can wrap my head around Java's data structures.

### Thanks
* [John Nunemaker](http://railstips.org/about/), for creating HTTParty
* [Luke Lowrey](http://lukencode.com/about), whose [RestClient](http://lukencode.com/2010/04/27/calling-web-services-in-android-using-httpclient/) class I borrowed many snippets from

