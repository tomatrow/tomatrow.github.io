---
title: The Coffeehouse Schema
layout: post
---

## Links 

* XML Parsing 
    - [Graphical xpath](http://www.qutoric.com/xslt/analyser/xpathtool.html)
    - [xmlstarlet](http://xmlstar.sourceforge.net)
* Our data
    - [The Map](https://www.google.com/maps/d/u/0/viewer?mid=1w3GLW8rs_d9xTbU8nQ52Bf5xwGo&ll=32.94402489959027%2C-117.14390477500001&z=8)
* Unix tools I need to use 
    - [sed vs awk](https://stackoverflow.com/questions/1632113/what-is-the-difference-between-sed-and-awk)
    - [grep ses and awk](https://stackoverflow.com/questions/7727640/differences-among-grep-awk-and-sed)


## Parsing the Map 


### The hard way 

The `xpath` expression `/kml/Document/Folder[name = "Coffee Shops"]/Placemark` gives us a list of `Placemark`s. Each `Placemark` has a `name`, `styleUrl`, and `Point` where `Point` has a single value `Coordinates`. A `Coordinates` is in the form `LATTITUDE, LONGITUDE, ?`. 

This would be perfectly fine if [namespaces]() didn't exist. 
So `xml sel -t -m '//_:Document/_:Folder[_:name = "Coffee Shops"]/_:Placemark' -c "." -n` *actually* works. See the [offcial troblueshooting guide](). 

To avoid the insanity above, delete the namespace and instead use `xml sel -t -m '/kml/Document/Folder[name = "Coffee Shops"]/Placemark' -c "." -n`

To get a big list of almost-what-we-want use 
`xml sel --text --noblanks -t -c '/kml/Document/Folder[name = "Coffee Shops"]/Placemark'`

```fish
 cat Coffee\ Shops2.xml # cat 
    | xml sel --text --noblanks -t -c '/kml/Document/Folder[name = "Coffee Shops"]/Placemark' \ # turns xml into plainish data 
    | sed 's/^ *//g' \ # removes leading space 
    | sed 's/\(.*\)#\(.*\)/{"name" : "\1", "color" : "#\2", /' \ # forms name/color pairs
    | sed 's/\(.*\),\(.*\),0/"lat" : \1, "lon" : \2 },/' \ # forms lat/lon pairs 
    | sed 's/[ \t]*$//'  \ # removes trailing space 
    | sed '/^\s*$/d' \ # removes whitespace lines 
    | ghead --bytes -2 \ # trims last two characters 
    | read -z json; # reads into json 
    and echo [$json] # finishes array syntax 
        | jq . # formatting 
        | subl # see it 
```


### The better way 


Note. We still need to remove the namespace. 


```fish
function parse -a xmlFile

    cat $xmlFile \
        | xml sel -t -c '/kml/Document/Folder[name = "Coffee Shops"]' \
        | xml fo \
        | read -z document

    set accumulator "[]"

    set i 1
    while true 

        # Set the place for this interation. 
        set place (echo $document | xml sel --template -c "/Folder/Placemark[$i]")
        # Check if we're done. 
        if not string length --quiet "$place"
            break
        end 

        # Build json from xml placemark. 
        set next "{}"
        for path in (string split " " "name styleUrl coordinates")

            echo $place \
                | xml sel --template --value-of "//$path" \
                | string trim \
                | read value 

            set anyRegex ".*"
            # Parse tag values into json objects. 
            if string match --quiet $path 'coordinates'
                # The coordinates still need more parsing. 
                # Note. We need to use `--` since there are negative latitudes.
                set pointRegex "-?\\\\d+\\\\.\\\\d+" # ugh
                echo "$value" \
                    | jq -R "capture(\"(?<lat>$pointRegex),(?<lon>$pointRegex),$anyRegex\") | .lat |= tonumber | .lon |= tonumber" \
                    | read -z part
            else 
                echo "$value" \
                    | jq -R "capture(\"(?<$path>$anyRegex)\")" \
                    | read -z part
            end 

            # Add this part to the object. 
            echo "$next" | jq ". + $part" | read -z next
        end 
        
        # Add this object to the list. 
        echo $accumulator | jq ". + [$next]" | read -z accumulator
        set i (math "$i + 1")

    end 

    echo $accumulator
end
```

We end up with something like...
```json
[
  {
    "name": "Copa Vida Carlsbad",
    "styleUrl": "#icon-1899-0F9D58-nodesc",
    "lat": -117.326501,
    "lon": 33.1266828
  },
  {
    "name": "Leap Coffee",
    "styleUrl": "#icon-1899-0F9D58-nodesc",
    "lat": -117.2733162,
    "lon": 33.1404887
  },
  ...
]
```



## Getting the rest of the data

Now we need to use the Google Maps API to get as mush data as we can about each shop. We have the name and coordiantes, we we can use the [Places API](https://developers.google.com/places/web-service/). Specifically we [attain a key](https://developers.google.com/places/web-service/get-api-key) use the [Place Search](https://developers.google.com/places/web-service/search) and then [Place Details](https://developers.google.com/places/web-service/details). 

### Get a Key
Script key
`AIzaSyCoRk_ztTGIlWYMfhH4pMHOzFUDugCyHEU`


### Place Search 


### Place Details