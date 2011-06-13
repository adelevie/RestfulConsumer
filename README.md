
RestfulConsumer
===============

Heavily inspired by Jon Nunemaker's HTTParty, RestfulConsumer tries to make API consumption as simple as possible--at least on the Java side of the world.

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

