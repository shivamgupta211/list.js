# List.js
Do you want a 6.899kb cross-browser native JavaScript that makes your plain HTML lists super flexible, searchable, sortable and filterable? **Yeah!**
Do you also want the possibility to add, edit and remove items by dead simple templating? **Hell yeah!**

# Super simple examples

More examples are found at [Listjs.com](http://listjs.com) and
[Listjs.com/examples.html](http://listjs.com/examples.html)

## Index existing list
HTML

    <div id="hacker-list">
        <ul class="list">                            
           <li>
               <h3 class="name">Jonny</h3>
               <p class="city">Stockholm</p>
           </li>                            
           <li>
               <h3 class="name">Jonas</h3>
               <p class="city">Berlin</p>
           </li>
        </ul>
    </div>

Javascript
 
    var templates = {
        valueNames: [ 'name', 'city' ]
    };
 
    var hackerList = new List('hacker-list', templates);

## Create list on initialization 
HTML

    <div id="hacker-list">
        <ul class="list"></ul>
    </div>
 
    <div style="display:none;">                            
        <!-- A template element is needed when list is empty, TODO: needs a better solution -->
        <li id="hacker-item">
           <h3 class="name"></h3>
           <p class="city"></p>
        </li>
    </div>
       
JavaScript
 
    var templates = {
        item: 'hacker-item'
    };
 
    var values = [
        { name: 'Jonny', city:'Stockholm' }
        , { name: 'Jonas', city:'Berlin' }
    ];
 
    var hackerList = new List('hacker-list', templates, values);


## Index existing list and then add
HTML

    <div id="hacker-list">
        <ul class="list">                            
           <li>
               <h3 class="name">Jonny</h3>
               <p class="city">Stockholm</p>
           </li>  
        </ul>
    </div>
       
JavaScript
 
    var templates = {
        valueNames: ['name', 'city']
    };
 
    var hackerList = new List('hacker-list', templates);
    
    hackerList.add( { name: 'Jonas', city:'Berlin' } );
 
## Add automagic search and sort inputs and buttons
HTML

    <div id="hacker-list">
        
        <input class="search" />
        <span class="sort" rel="name">Sort by name</span>
        <span class="sort" rel="city">Sort by city</span>
        
        <ul class="list">                            
           <li>
               <h3 class="name">Jonny</h3>
               <p class="city">Stockholm</p>
           </li>                            
           <li>
               <h3 class="name">Jonas</h3>
               <p class="city">Berlin</p>
           </li>
        </ul>
    </div>

Javascript (nothing special)
 
    var templates = {
        valueNames: [ 'name', 'city' ]
    };
 
    var hackerList = new List('hacker-list', templates);


# Documentation 

## Create a list

### Constructor
*	List(id, options, values)

### Parameters
* **id** *(\*required)*  
 Id the element in which the list area should be initialized.
* **options**  
Some of the option parameters are required at some times
	* **templates** _(Object)_
		* **valueNames** _(Array, default: null) (*only required if list already contains items before initialization)_   
		If the list contains items on initialization does this array
		have to contain the value names (class names) for the different values of 
		each list item.
		    
		        <ul class="list">
		            <li>
		                <span class="name">Jonny</span>
		                <span class="city">Sundsvall</span>
		            </li>
		        </ul>
		        
		        var valueNames = ['name', 'city'];
		    
		* **item** _(String, default: undefined)_  
		ID to item template element 
	* **indexAsync** _(Boolean, default: false)_  
	If there already are items in the list to which the 
	List.js-script is added, should the indexing be done 
	in a asynchronous way? Good for large lists (> 500 items).
* **values** _(Array of objects) (*optional)_  
Values to add to the list on initialization.


## List API
These methods are available for the List-object.

### Attributes
* **listContainer** _(Element)_  
The element node that contains the entire list area.
* **items** _(Array)_  
A Array of all Item-objects in the list.
* **list** _(Element)_  
The element containing all items.
* **templateEngines** _(Object)_  
Contains all template engines available.

### Functions
* **add(values, options)**  
Adds one or more items to the list. 
	* values
	* options
* **addAsync(values, options)**  
Adds one or more items the the list in a asynchronous way.
	* values
	* options
* **remove(valueName, value, options)**  
Removes items from the list where the value named "valueName" has value "value". 
Returns the count of items that where removed.

		itemsInList = [
			{ id: 1, name: "Jonny" }
			, { id: 2, name "Gustaf" }
		]
		listObj.remove("id", 1); -> return 1

* **get(valueName, value)**  
Returns values from the list where the value named "valueName" has value "value".
	* valueName
	* value  
	
			itemsInList = [
				{ id: 1, name: "Jonny" }
				, { id: 2, name "Gustaf" }
			]
			listObj.get("id", 2); -> return { id: 2, name "Gustaf" }

* **sort(valueName, sortFunction)**  
Sorts the list based in values in column named "valueOrEvent". The sortFunction 
parameter is used if you want to make you one sort function.
	* valueOrEvent
	* sortFunction

* **search(searchString, columns)**  
Searches the list 	

* **filter(filterFunction)**  
	* filterFunction
* **size()**  
Returns the size of the list


## Item API 
These methods are available for all Items that are returned by
the list.
### Attributes

* **elm** _(Element)_  
The actual item DOM element

### Functions
* **values(newValues)**
	* newValues _optional_
	If variable newValues are present the new values replaces the current item values
	and updates the list. 
	If newValues are not present, the function returns the current values.
	
	        item.values() -> { name: "Jonny", age: 25 }
	        item.values({
	            age: 26,
	            city: "Sundsvall"
	        });
	        item.values() -> { name: "Jonny", age: 26, city: "Sundsvall" }
	    
* **show()**
Shows the item 
* **hide()**
Hides the item (removes the element from the list, and then when its shown its appended again, the element will thereby change position in the list, bug, but a good solution is yet to find)


## TemplateEngine API 
Only needed if you want to build you own template engine

### Attributes

None

### Functions
* **get(item, valueNames)** 

* **set(item, values)** 

* **create(item)** 

* **add(item)** 

* **remove(item)** 

* **show(item)** 

* **hide(item)** 


## Helper functions
Called by ListJsHelpers.functionName()

* **getByClass()** 

* **addEvent()** 

* **getAttribute()** 

## How to build the script
Type just *ant* in the console while in root folder.

# Changelog
### 2011-10-18 Beta 0.1 release
* Examples at Listjs.com works in IE7,8,9 (IE6 is not tested, should work)
* More documentation
* Misc bug fixes

### 2011-10-15 Final alpha 0.3 release
* More documentation
* Only show 200 items at same time, huge speed increase
* Misc bug fixes

### 2011-08-08 Alpha 0.2 release
* Added asynchronous item adding
* Added asynchronous list indexing
* Improved (but incomplete) documentation
* Bugfixes and improved helper functions
* Show helper functions non-minified

### 2011-07-25 Alpha 0.1 release