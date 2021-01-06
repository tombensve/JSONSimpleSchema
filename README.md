# Note

Since I'm using Groovy a lot and Groovy allows me to initialize Map with similar structures as JS JSON, and I could
then generate JSON from the Maps, I though it would be nice to have frontend and backend work / look similar in its
data handling. 

**I have decided that this was a bad idea!** 

I'm going to replace such stuctures with POJO models
and serialize to / from JSON. Then this is not needed. And Groovy is super simple when it comes to creating java beans
since it automatically creates getters and setters.

I'm not removing this if someone is of different opinion and want to use this or build on it, but I'm not doing anything more with it myself.

# JSONSimpleSchema

A very simple way of validating JSON structures.

I know that there is an official JSON schema project out there. I've even tried it. Even a relatively simple JSON structure required quite of lot of JSON code in the schema. I only covered 25% of my JSON document before I gave up on the then quite large JSON schema. 

I decided I wanted something much simpler. Something that did not support 100% of all possible JSON structures, but could handle reasonable simple JSON structures. So I came upp with this:

(do not expect example to have realistic data, nor make sense in any way. Its the format that is important):
 
    schema = {
        header_?: "The nessage header.",
        header_1: {
           type_1      : "service",
           address_1   : ".admin.web",
           classifier_0: "?public|private"
        },
        body_1  : {
           action_1: "get-webs"
        },
        reply_0: {
           webs_1: [
              {
                 name_1: "?.*",
                 url_0: "?^https?://.*",
                 someNumber_0: "#0-100" // Also valid:  ">0" "<100" ">=0" "<=100"
              }
           ]
        },
        addrmap_1: {
           {
              "name_1": "?[a-z,A-z,0-9,_]*",
              "value_1": "?[0-9,.]*"
           }
        }
    }
 
## Keys
 
__key\_1__ : This entry is required.
__key\_0__ : This entry is optional.
__key_?__  : A description of this key.
 
__Note__ that all keys must always be static!
 
## Values
 
### "?regexp"
 
The '?' indicates that the rest of the value is a regular expression. This regular expression will be applied to each value.
 
### "\#range"

This indicates that this is a number and defines the number range allowed. The following variants are available:

__"#from-to"__ : This specifies a range of allowed values, from lowest to highest.

__"#<=num"__ : This specifies that the numeric value must be less than or equal to the specified number.

__"#>=num"__ : This specifies that the numeric value must be larger than or equal to the specified number.

__"#<num"__ : This specifies that the numeric value must be less than the specified number.

__"#>num"__ : This specifies that the numeric value must be larger than the specified number.

Note: Both floating point numbers and integers are allowed.

### "!"

This means the value is a boolean.

### "bla"

This requires values to be exactly "bla".

### [:]

An empty object means any object, non validated. Do note that it is not possible to have validated objects
under a non validated object. When there is a non validated object, anything under it is also non validated.

# About this repo

This was orignally written in Groovy, and then I ported it to Javascript also. It is part of another project of mine. I decided to break this out into its own repo since it is completely general. 

This repo does currently not build any jars nor npm modules, etc. The code is small and simple just copy it into your project if you want to use it. 

I'll happily accept pull requests for improvements and ports to other languages.
