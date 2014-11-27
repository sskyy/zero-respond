# zero-respond #

This module helps you handle respond when you use bus in your request handler.

## Usage ##

1. Add dependency to your module package.json file like:

```
{
	"name" : "YOUR_MODULE_NAME",
	"zero" : {
		"dependencies" : {
			"respond" : "^0.0.1"
		}
	}
}
```

2. Set data named `respond` in your request' bus.
Respond module will add a router handler which will always be called at last of every request,
and read `bus.data('respond')` to decide what to respond.

```
module.exports = {
    route : {
        "GET /fcall" : function( req, res){
            //bus.fcall will help you fire event before and after your main business logic
            req.bus.fcall("someEvent", arg1, arg2, function(){
                //deal what you want to do

                //respond module can handler 3 type of output, `data`, `file` and `page`.

                //1. tell respond module to output data as json.
                req.bus.data("respond.data",{"key":"value})

                //2. tell respond module to render page with data, and output result html.
                req.bus.data("respond.data", DATA_FOR_PAGE_RENDERING)
                req.bus.data("respond.page", TEMPLATE_LOCATION)

                //3. tell respond module to send file
                req.bus.data("respond.fire", FILE_LOCATION)
            })
        }
    }
}
```

We strongly suggest you use respond module to handle respond for you rather then you do it yourself,
because In this way other module can change of attach extra data to respond data easily.
And you don't need to be worried about your respond data being messed by other module, because bus will trace all data modification which you can check via dev tool.
