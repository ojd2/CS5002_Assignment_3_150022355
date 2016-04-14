### Program Overview
The following code involved a lot of intricate and complex procedures in order to fulfill the practical requirements. There were a number of different methods and algorithms to be used in order to efficiently extract data into the HTML table. My main aim for this practical was to explore iterative and decomposable algorithms that could be used together. The first aim of the code is to establish and find both ```Observation Time``` and ```Observation Date``` in the weather data file. If this could be executed in a small amount of runtime, it would enable the program to perform and execute the next set of instructions efficiently. Subsequently, there are two global variables initiated at the beginning of the code in order to be used as storage for a selected date value and another to store all data associated with the selected date value.
```JavaScript
// global variables to be used in functions. var selectedDate;
var data_matches;
```
After analysing the data file, it became apparent that the data contained over 6000 records. Therefore, the most important objective was to construct an iterative array search algorithm that would be quick in runtime. The algorithm identifies if the data has a desired key index value, then extracts the associated objects into an empty array. The first function called ```getDate()``` does just this:

```javascript
// getDate() to iterate through Observation Dates in array of data.
  function getDate() {
    // set up variables for search.
    var date = "Observation Date";
    var dateFound = false;
    var new_dates = [];
    // set up generic loop.
    // will have to change back to array.length() instead of 10.
    for (var i = 0; i < weatherData.length; i++) {
      // object set for array.
      var object = weatherData[i];
      // for dateProperty in object begin iteration. 
      for (var dateProperty in object) {
        //console.log('item ' + i + ' : ' + dateProperty + ' = ' + object[dateProperty] + "\n");
        if (dateProperty == date) {
          dateFound = true;
          var dateResults = object[dateProperty].split(',');
          // push elements into new_dates.
          // call console.log outside initial for loop for testing.
          new_dates.push(dateResults);
        }
      }
    }
```
The function starts with three variables that store a desired key index value as a string to search and compare with. After this, there is a test variable set to false and an empty array to store our searched and identified objects. Next we begin the algorithm with a for loop that iterates over the data file’s 
```javascript
array weatherData[].
```
Next another for loop is initiated to begin a deeper iteration through each object contained within the ```weatherData[]``` array. However, the loop is a for in loop that uses the date variable as a search algorithm. 

If any of the objects contain a key ```index``` value of ```Observation Date``` – each object must be split and then stored into an empty array called ```new_dates```. A number of console logs and comments were added to the code in order for a backend perspective of the code and its operations. This made it easier to establish what was efficiently working and what was not, during the debug stage.

The ```sortDate()``` function subsequently takes the array new_dates, containing all dates found in the weatherData array and sorts the dates from lowest to highest. The function contains a simple ```.sort()``` JavaScript method:
```javascript
var sortedDates = new_dates.sort(function(a, b) { 
   return Date.parse(a) > Date.parse(b);
});
```

Next the sorted array new_dates now has all dates extracted in order and added to the select box for the

HTML element “daySelect”. The following code looks like:
```javascript
// add minDate to select dropDown.
var minDateSelect = document.getElementById("daySelect"); var minOption = document.createElement("option"); minOption.text = minDate;
minOption.value = minDate;
minDateSelect.add(minOption);
// add maxDate to select dropDown.
var maxDateSelect = document.getElementById("daySelect"); var maxOption = document.createElement("option"); maxOption.text = maxDate;
maxOption.value = maxDate;
maxDateSelect.add(maxOption);
```

Two core aspects of the code are detrimental to the functionality and composition of the code. Two select dropdowns have been made available according to the practical documentation highlighted. Both of these HTML select dropdowns should contain the values of the ```Observation Date``` and ```Observation Time``` from the provided data. As you can see from the example code above, the data has been iterated through and firstly - ```Observation Date``` has been extracted and sorted from highest too lowest. From here the code makes use of JavaScript’s ```getElementById``` and ```createElement``` methods for adding HTML DOM elements in real-time. It stores both the lowest and highest orders of dates found and appends them as select options. Next, there is an onchange JavaScript function that captures the value of both the select options early on:
```javascript
// capture value changes on change for select options. 
document.getElementById("daySelect").onchange = function() {
// clearSelect() for clearing time dropdown options. clearSelect();
// append dropdown options for chosen date.
selectedDate = document.getElementById("daySelect").value; console.log(selectedDate);
var select = document.getElementById("timeSelect"); var length = select.options.length;
```

For example, for any chosen date, the code stores its value and begins another for loop to be used for appending all ```Observation Times``` in the data for the desired date value selected:

```javascript
for (i = 0; i < length; i++) { select.options[i + 1] = null;
}
// call getTimes() getTimes();
}
```
The ```getTimes()``` function is also called at this point. This ultimately initiates the same functionality as the as the ```getDates()``` function however, the data extracts all the values of the index key ```Observation Time``` instead:

```javascript
// getTimes() to iterate through Observation Times in array.
function getTimes() {
    // set up variables for search.
    var time = "Observation Time";
    var timeFound = false;
    var new_times = [];
    // set up generic loop.
    // will have to change back to array.length() instead of 10.
    for (var i = 0; i < weatherData.length; i++) {
      // object set for array.
      var object = weatherData[i];
      // for timeProperty in object begin iteration. 
      for (var timeProperty in object) {
          if (timeProperty == time) {
            timeFound = true;
            var timeResults = object[timeProperty];
            // push elements into new_times.
            // call console.log outside initial for loop for testing.
            if (object["Observation Date"] == selectedDate) {
              new_times.push(timeResults);
            }
          }
      }
 }
 console.log(new_times);
```

The results are stored into the empty array ```new_times``` if and only if the data extracted is equal to the time value selected by the user. It seemed logical to make the extracted times append to the select dropdown box and be only the times for either the 28th or 29th, but not display all the times at once. At first, the code presented was appending multiple instances of the same ```Observation Time``` found in the weather data file. As a consequence, an algorithm was needed to find all instances of the same value and only extract just the one value. For example, if the data has 20 values for the ```Observation Time``` of ```23:00``` – the algorithm must only extract just the one value. This must then be repeated for all multiple instances of the values. The algorithm must have the functionality to add the sorted values and singular values into an empty array. From this the function devised was based on an algorithm suggested by a user on Stack Overflow (Stack Overflow, 2011) that counts all the multiple occurrences of array elements and only stores the one element:

The simple algorithm looks like the following:

```javascript
// sortTimes() to sort on new_times.
// finds all instances of array element.
// see: http://goo.gl/Bjitig for function
function sortTimes() {
    var a = [],
    b = [],
    prev;
    new_times.sort();
    for (var j = 0; j < new_times.length; j++) {
      if (new_times[j] !== prev) {
        a.push(new_times[j]);
        b.push(1);
      } else {
        b[b.length - 1]++;
      }
      prev = new_times[j];
    }
      return [a, b];
}
```
The variable ```timeResults``` stores the returned elements of the function ```sortTimes(new_times)```. After this, the code begins to extract the returned elements of the ```obsTimes``` variable and begins to append the array elements as HTML select ```<option>``` elements:

```javascript
// store results in obsTimes[]
var obsTimes = timeResults[0];
// function to separate the obsTimes[] array.
// add obsTimes[] array of times to select dropDown. 
var timeSelect = document.getElementById("timeSelect"); 
var fragment = document.createDocumentFragment();
// add sorted array to select options. 
obsTimes.forEach(function(obsTimes, index) {
var timeOption = document.createElement("option"); timeOption.innerHTML = obsTimes;
  timeOption.value = obsTimes; fragment.appendChild(timeOption);
}); 
timeSelect.appendChild(fragment);
```

It seems as though using the fragment method for appending ```select``` options with individual array elements at the same time was the most efficient method. At first, without the fragment method, all the array elements were appending into the one ```select <option>```. Another reason for using it, was because the fragment method enables more fluid functionality for removing and adding other various elements easily further down the line.

The next block in the code is a crucial as it is detrimental to the rest of the programs functionality. Because of the vast quantity of times available for both date values, it made logical sense to add an ```<button>``` to the HTML that could be used as a pointer function. For example, once the user has selected a date and time from the two dropdowns, once the button is clicked – a function will crawl the original data and pull out all objects associated with both time and date (such as 28th and 4:00PM).

Once the button is clicked, it performs four functions and additionally captures any select values that have been changed from the two HTML dropdowns:

```javascript

// function onclick for when user selects date and time.
document.getElementById("data_btn").onclick = function() {
// call clearTable()
clearTable();
    
// store time index and text from select dropDowns.
var timeIndex = document.getElementById("timeSelect").selectedIndex; 
var timeText = document.getElementById("timeSelect").options;
var selectedTime = timeText[timeIndex].text.toString();
console.log("Selected time is: " + selectedTime);
    
// store date index and text from select dropDowns.
var dateIndex = document.getElementById("daySelect").selectedIndex;
var dateText = document.getElementById("daySelect").options;
var selectedDate = dateText[dateIndex].text.toString();
console.log("Selected date is: " + selectedDate);
    
// console log function find matches for both time and date select values.
console.log(findMatches(selectedTime, selectedDate));
// call appendMinMax() 
appendMinMax();
// call appendWeatherData() 
appendWeatherData();
// call rowColor()
rowColor();
}
```

The order of the function callbacks is very important and took some time to sort them into logical ordering. First, there is a function called to clear the 11 column HTML table once every new date and time has been selected and a user clicks the button. Some variables are declared to store the date and time select values to be used in the ```findMatches()``` function that subsequently comes in the next block. The ```findMatches()``` function takes the two stored select values as parameters and runs a simple search and match function on the weather data array. The first quarter of the programs code, is for populating both the date and times available into the select dropdowns. The next quarter of the code is designed to take the two date and time values upon click and run queries on the weather data accordingly. For every object in the weather data file that contains the selected date and selected time, the objects are extracted and pushed into a variable called ```data_matches```.
The ```findMatches()``` function can be seen below and the clearTable() function can be seen further on in the report:
```javascript
// findMatches() to find matches based on selected for date and time.  
function findMatches(time, date) {
    data_matches = [];
    var foundMatch = false;
    for (var j = 0; j < weatherData.length; j++) {
     if (weatherData[j]["Observation Time"] == time && weatherData[j ["Observation Date"] == date) {
      // push into global array, set at top of file.
      data_matches.push(weatherData[j]);
     }
   }
    console.log(data_matches);
}
```
The following two functions that come next are for finding the lowest and highest values per key index in the ```data_matches``` array:

```javascript
// findMin() for finding MINIMUM
function findMin(key) {
    var data_min = [];
    var data_max = [];
    var min = 100;
    var ignore = -99.00;
    var minObject;
      
    for (var x = 0; x < data_matches.length; x++) {
      //console.log("KEY: " + weatherData[x][key]);
      if (parseFloat(data_matches[x][key]) < min && parseFloat(data_matches[x][key]) != ignore) {
        min = parseFloat(data_matches[x][key]);
        console.log(data_matches[x]);
        minObject = data_matches[x];
      }
    }
    console.log("Min value = " + min);
    console.log(minObject);
    return minObject;
}

// findMax() for finding MAXIMUM
function findMax(key) {
    var data_min = [];
    var data_max = [];
    var max = 0;
    var maxObject;
       
    for (var x = 0; x < data_matches.length; x++) {
      //console.log("KEY: " + weatherData[x][key]);
      if (parseFloat(data_matches[x][key]) > max) {
        max = parseFloat(data_matches[x][key]);
        console.log(data_matches[x]);
        maxObject = data_matches[x];
      }
    }
    console.log("Max value = " + max);
    console.log(maxObject);
    return maxObject;
}
```

The first minimum finds all values under 100, that is an arbitrary number as there seemed to be no numbers above 100 for both ```Screen Temperature``` or ```Wind Speed```. A variable called min stores the value of 100 and any data_match object less than 100, is searched and compared until the lowest value. The same is done for finding the maximum however, this time the ```data_match``` object must have a value greater than 0 and is compared until the highest value.

Next ```appendMinMax()``` is called next within the onclick function above. This function takes the highest and lowest values from the associated objects and the extracts the additional values: ```Site Name```, ```Region``` to be appended to the associated table column ids. The function looks elongated, but essentially it contains some global variables that perform the functions of ```findMax()``` and ```findMin()``` on the chosen array key indexes:

```javascript
// appendMinMax() function.
  function appendMinMax() {
    // call min functions for "Wind Speed" and "Screen Temperature".
    minWindSpeed = findMin("Wind Speed");
    console.log(minWindSpeed["Site Name"] + " " + minWindSpeed["Region"]);
    minScreenTemp = findMin("Screen Temperature");
    console.log(minScreenTemp["Site Name"] + " " + minScreenTemp["Region"]);
        
    // call max functions for "Wind Speed" and "Screen Temperature".
    maxWindSpeed = findMax("Wind Speed");
    console.log(maxWindSpeed["Site Name"] + " " + maxWindSpeed["Region"]);
    maxScreenTemp = findMax("Screen Temperature");
    console.log(maxScreenTemp["Site Name"] + " " + maxScreenTemp["Region"]);
        
    // get min and max for screen temperature.
    var min_screen = [minScreenTemp["Screen Temperature"], minScreenTemp["Site Name"], minScreenTemp["Region"]];
    var max_screen = [maxScreenTemp["Screen Temperature"], maxScreenTemp["Site Name"], maxScreenTemp["Region"]];
        
    // get min and max for wind speed.
    var min_wind = [minWindSpeed["Wind Speed"], minWindSpeed["Site Name"], minWindSpeed["Region"]];
    var max_wind = [maxWindSpeed["Wind Speed"], maxWindSpeed["Site Name"], maxWindSpeed["Region"]];
        
    // add array data to minmax table.
    // for loop to iterate through variables above and add each [i] to innerHTML of <td> cell.
    for (var i = 0; i < 3; i++) {
      document.getElementById("minScrTemp").getElementsByTagName("td")[i + 1].innerHTML = min_screen[i];
      document.getElementById("maxScrTemp").getElementsByTagName("td")[i + 1].innerHTML = max_screen[i];
      document.getElementById("minWindSp").getElementsByTagName("td")[i + 1].innerHTML = min_wind[i];
      document.getElementById("maxWindSp").getElementsByTagName("td")[i + 1].innerHTML = max_wind[i];
    }
}
```
The for loop iterates through each column and applying the desired minimum and maximum object and its values to each row. Once the ```appendMinMax()``` function is executed, the ```appendWeatherData()``` function is called and subsequently extracts data from the data_matches array we have been using. The ```data_matches``` array contains all data associated with the selected date and time values. Therefore, all the data is extracted and added to the 11 column based table. The procedure for extracting / appending the desired index keys and their values to the table, was far more difficult than first thought. First, the data had to be sorted alphabetically using the column key ```Site Name```. To begin with, the ```appendWeatherData()``` function uses a simple compare and sort algorithm:

```javascript
// appendWeatherData() for appending all data_matches to table.
  function appendWeatherData() {
    // sort alphabetically, begin by comparing "Site Name" index key.
    function compareStrings(a, b) {
      // convert to lowercase comparison.
      a = a.toLowerCase();
      b = b.toLowerCase();
      return (a < b) ? -1 : (a > b) ? 1 : 0;
    }
    // return sort() function.
    data_matches.sort(function(a, b) {
    return compareStrings(a["Site Name"], b["Site Name"]);
    });
```
 
The function ```compareStrings()``` executes at the top of the ```appendWeatherData()``` function and as a consequence, sorts the data before it is appended to the HTML table. The ```sort()``` JavaScript method is called using the parameters (a, b) on data_matches using the parameters and index key ```Site Name```. This then compares all values in the associated index key and sorts them alphabetically. After this is executed, the ```data_matches``` array must be appended to the HTML table.

For appending data to the main HTML table element, the idea was to create separate functions to individually add a ```<tr>``` row and inside the same row add ```<td>``` cells for 11 columns. Initially, the code was very complex and needed to be shortened. Issues were arising when using multiple for loops to manually add rows and cells. Data was being appended into the HTML table but only into one row. A cleaner and more concise method was required. From reading the following article (Redips, 2015) on adding table rows and columns, adding a row and adding a cell were both done separately. They both make use of the ```insertRow()``` and ```inserCell()``` JavaScript method to appended text from ```data_matches```. First, a row is added to the table and within this function, a for loop is used to count and add 10 cells to the row. From here another small for loop is initiated to append a row for every ```data_matches``` object. Essentially, every time ```appendRow()``` function is called, the ```createCell()``` function is also called and they move integrate and append data values as text nodes from the data_matches array of associated objects. This approach is more seamless compared to the original code. Runtime also seems to run considerably more efficient and faster.

The code for the ```appendRow()``` function looks like the following:

```javascript
// appendRow() function to append row to the HTML table.

function appendRow(data) {
      // fetch table reference.
      var table = document.getElementById("weatherData"),
      // append table row to reference above. 
      row = table.insertRow(table.rows.length), 
      i;
      // insert 10 table cells to the new row.
      for (i = 0; i < 11; i++) {
      createCell(row.insertCell(i), data, i);
      }
    }
        
    // finds all data_matches and runs appendRow() for each object.
    for (i = 0; i < data_matches.length; i++) {
    // calls appendRow();
    appendRow(data_matches[i]);
}
```
The code for the ```createCell()``` function being called looks like the following:
```javascript
// createCell() function
function createCell(cell, text, x) {
      var column_indexes = [7, 1, 6, 5, 2, 3, 8, 9, 0, 4, 10];
      var column_names = ["Pressure", "Site Name", "Screen Temperature","Wind Speed", "Pressure Tendency", "Significant Weather","Region", "Observation Time", "Wind Gust", "Wind Direction","Visibility"];
      var content_key = text[column_names[column_indexes[x]]];
      var container = document.createElement('span');
      
      if(x == 4 && content_key == minScreenTemp["Screen Temperature"]) {
      container.style.color = "red";
      }
      if(x == 5 && content_key == minWindSpeed["Wind Speed"]) {
      container.style.color = "red";
      }
      if(x == 4 && content_key == maxScreenTemp["Screen Temperature"]) {
      container.style.color = "green";
      }
      if(x == 5 && content_key == maxWindSpeed["Wind Speed"]) {
      container.style.color = "green";
      }
      // create text node to extract from object.
      txt = document.createTextNode(JSON.stringify(content_key).replace(/"/g, "")); 
      // append text node inside the container.
      container.appendChild(txt);
      // append container to the table cell. 
      cell.appendChild(container); 
      
    }
}
```
This function is a little more complex in terms of structure. Firstly, it takes three parameters called: ```cell```, ```text```, and ```x```. The first parameter is used as a container to wrap the text nodes for values inserted into each cell wrapped with a ```<span>``` HTML tag. The parameter ```text``` is used for some conditional statements and for extracting all data that matches the ```column_indexes``` and ```column_names``` arrays. At first, the ordering of the data into columns was being executed back to front. After numerous attempts, it seemed as though it was easier to create two arrays that store the index keys as strings and another that stores integers ```0 – 10```.

From here, the ```x``` parameter is used as an incremental counter. The variable content_key stores the text node value of both column_names and column_index with the ```x``` counter. From numerous attempts of reordering, it seems as though if the column_indexes and the column_names are matched according to their column order in the HTML table, the ```container.appendChild(txt)``` extracts the correct values. For example, column_name of “Pressure” is applied to ```column_indexes``` of 7, that represents column 7 to contain the text value of Pressure for each object in ```data_matches```.

What follows next, are simple conditional if statements that ask if the parameter and our counter ```x``` reaches either column 4 or column five and is equal to our global variables: ```minScreenTemp```, ```minWindSpeed```, ```maxScreenTemp```, ```maxWindSpeed```, then for each ```min``` and ```max``` – change the font colour of the appended value either green or red. The following code contains a variable called ```txt``` that stores the ```createTextNode``` JavaScript method that uses the ```content_key```. It uses content_key because the variable contains the “text” parameter and the correct ordering of appending object values to the desired HTML table columns.

Once both the ```appenedRow()``` and ```createCell()``` functions are executed, immediately afterward comes the ```rowColor()``` function. The function iterates over all of the table rows for any given date and time selected. Next it simply applies HTML style elements for background and font colour and for every second row and the background colour applied is grey. The first for loop is only applied to the first row that contains the column titles.

Finally, there is are the functions ```clearTable()``` and ```clearSelect()```. Both functions are called onclick and onchange further up in the program. This simply replaces the inner HTML of the table element with just the first row of column titles. The original ```clearTable()``` function was clearing the whole table including the first row of column titles – that subsequently made the page go blank after every click and select. From a user experience point of view, adding the ```innerHTML``` element allows the first row to always be present upon click. The ```clearSelect()``` function follows the same principle, but is executed upon the select dropdown for times. Once the chosen date changes value, the ```clearSelect()``` function is called and then immediately afterwards the desired times for the date are appended as new select options.
