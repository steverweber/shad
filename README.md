Shad
----

Small framework for building RESTful API wrappers.

Install
-------


Usage
-----

Define your base API class

    from shad import BaseAPI, APIFunction, get_bind
    
    class MyAPI(BaseAPI):
        def __init__(self, api_key, base_url="http://example.com/"):
           self.api_key = api_key
           self.base_url = base_url
        
   		def update_parameters(self, params):
   		   params["key"] = self.api_key
   		   return params
   		   
`base_url` is required, and used by the method `get_base_url` to build the requests url. You can override `get_base_url` if you want to include variable constants in your base url in every request.

Next define your end points

	bind = get_bind(MyAPI)

    @bind
    class mycall(APIFunction):
        path = "accounts/login/"
        method = "GET"
        
Then a user of your wrapper can do this:

    api = MyAPI('THISISMYAPIKEY')
    r = api.mycall(format='json')
    
By default, call return insteads of the Python requests libraries response objects, which you can call `.json` on to get back json if that is what you expect.

	r.json
	{'data':'awesome'}
	
This breaks the paradigm of just calling function and getting back a workable response, I need to change this such that we return an object which is by default the data in json form (or xml form), and you can also get the reponse back too.
        