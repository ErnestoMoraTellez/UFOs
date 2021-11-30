# UFOs

## Overview of UFOs

We are helping Dana. She will be writing about her home town McMinnville. The topic that she choose UFOs. She has a Javascript database with a lot of information of this topic, and she is looking for a way to display it.

## Purpose

We will be using javascript to create a table to visualize data. At the end we will create a HTML page to show our work about UFO sightings with some filters.

## UFOs result

First we have a lot of date in a data.js file. Here is an example of the information that we can find.

    var data = [
      {
        datetime: "1/1/2010",
        city: "benton",
        state: "ar",
        country: "us",
        shape: "circle",
        durationMinutes: "5 mins.",
        comments: "4 bright green circles high in the sky going in circles then one bright green light at my front door."
      },
      {
        datetime: "1/1/2010",
        city: "bonita",
        state: "ca",
        country: "us",
        shape: "light",
        durationMinutes: "13 minutes",
        comments: "Three bright red lights witnessed floating stationary over San Diego New Years Day 2010"
      },

We needed to create an other js file to manipule data and create the filters.

First we need to import the date from the other file.

    // from data.js
    const tableData = data;

    // get table references
    var tbody = d3.select("tbody");

Then we create a function to build the table using the infomration given by the data.js or the filters that we select.

    function buildTable(data) {
      // First, clear out any existing data
      tbody.html("");

      // Next, loop through each object in the data
      // and append a row and cells for each value in the row
      data.forEach((dataRow) => {
        // Append a row to the table body
        let row = tbody.append("tr");

        // Loop through each field in the dataRow and add
        // each value as a table cell (td)
        Object.values(dataRow).forEach((val) => {
          let cell = row.append("td");
          cell.text(val);
        });
      });
    }

As next important point, we create a function to get the options of the user in the webpage and use it to filter the information to create a new table.

    // 1. Create a variable to keep track of all the filters as an object.
    var filters = {};

    // 3. Use this function to update the filters. 
    function updateFilters() {

        // 4a. Save the element that was changed as a variable.
      let changedElements = d3.select(this);
        // 4b. Save the value that was changed as a variable.
      let elementValue = changedElements.property("value")
      console.log(elementValue);
        // 4c. Save the id of the filter that was changed as a variable.
      let filterId = changedElements.attr("id")
      console.log(filterId)

        // 5. If a filter value was entered then add that filterId and value
        // to the filters list. Otherwise, clear that filter from the filters object.
      if (elementValue){
        filters[filterId] = elementValue;
        console.log(filters)
      }

      else{
        delete filters[filterId]
      }

        // 6. Call function to apply all filters and rebuild the table
        filterTable(filters);

      }

As next function, when we have the filters, we use fiterTable function to filter the information that matches the user selection. Then we return this info to buildTable function to update the table in the webpage.

      // 7. Use this function to filter the table when data is entered.
    function filterTable(filters) {

        // 8. Set the filtered data to the tableData.
      let filteredData = tableData;  
        // 9. Loop through all of the filters and keep any data that
        // matches the filter values
        if (filters) {
          Object.keys(filters).forEach((id) =>{
            var key = id;
            filteredData = filteredData.filter(row => row[id] === filters[id]);
          });
        }

        // 10. Finally, rebuild the table using the filtered data
        buildTable(filteredData);
      }

      // 2. Attach an event to listen for changes to each filter
    d3.selectAll("input").on("change",updateFilters);

      // Build the table when the page loads
      buildTable(tableData);

For the visualization, we crear a HTML file with 4 filters, by date, city, country, state and shape. We can find the script next. We give the page some stile and create the tables to show the data.

    <!DOCTYPE html>

    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
    </head>
    <link
          rel="stylesheet"
          href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css"
          integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm"
          crossorigin="anonymous"
        />
    <link rel="stylesheet" href="static/css/style.css">
    <html lang="en">

    <<body class="bg-dark">
        <div class="wrapper">
            <nav class="navbar navbar-dark bg-dark navbar-expand-lg">
                <a class="navbar-brand" href="index.html">UFO Sightings</a>
            </nav>
        </div>
        <div class="jumbotron">
            <h1 class="display-4">The Truth is Out There</h1>
        </div>
        <div class="container-fluid">
            <div class="row">
                <div class="col-md-4">
                    <h3>UFO Sightings: Fact or Fancy?
                        <small>Ufologists Weigh In</small></h3>
                </div>
                <div class="col-md-8">
                    <p>
                        Are we alone in the universe? For millennia, humans have turned to the sky to answer this question. Now, thanks to research generously funded by W. Avy, a UFO-enthusiast and amateur ufologist, we can supplement our sky-searching with data analysis.<br><br>"The release of this analysis is well-timed: It coincides with the celebration of World UFO Day, which is a moment for ufologists around the world to connect, relax, and sample a range of UFO-themed snacks," said Dr. Ursula F. Olivier, the world's preeminent expert on circular sightings. "Citizen-scientists can be especially helpful in both cataloguing sightings—which is hugely helpful for us in our search for aliens—and in helping us celebrate the work that has already been done, such as this data visualization project, which will help us raise awareness of the ubiquity of sightings!"<br><br>Not everyone is ready to celebrate, however. Local CEO and vocal anti-alien activist V. Isualize reached out to our reporters to go on record as firmly opposed to any attempts to provide access to this data. "If there are aliens, they certainly would like to be left alone," she stated, before directing us to the Leave Aliens Alone (LAA) community engagement initiative she founded and funds.<br><br>So what do YOU think? Are we alone in the universe? Are aliens trying to contact us, or do they want to be left alone? Dig through the data yourself, and let us know what you see.      
                    </p>
                </div>
            </div>
        </div>
        <div class="container-fluid">
            <div class="row">
              <div class="col-md-3">
                <form class = "filters">
                    <p>Filter Search</p>
                    <ul class="bg-dark">
                        <li class="list-group-item bg-dark">
                            <label for="date">Enter Date</label>
                            <input type="text" placeholder="1/10/2010" id="datetime" />
                        </li>
                        <li class="list-group-item bg-dark">
                            <label for="City">Enter a City</label>
                            <input type="text" placeholder="roswell" id="city" />
                        </li>
                        <li class="list-group-item bg-dark">
                            <label for="State">Enter a State</label>
                            <input type="text" placeholder="ca" id="state" />
                        </li>
                        <li class="list-group-item bg-dark">
                            <label for="Country">Enter a Country</label>
                            <input type="text" placeholder="us" id="country" />
                        </li>
                        <li class="list-group-item bg-dark">
                            <label for="Shape">Enter a Shape</label>
                            <input type="text" placeholder="circle" id="shape" />
                        </li>
                    </ul>
                </form>
              </div>
              <div class="col-md-9">
                <table class="table table-striped">
                  <thead>
                    <tr>
                        <th>Date</th>
                        <th>City</th>
                        <th>State</th>
                        <th>Country</th>
                        <th>Shape</th>
                        <th>Duration</th>
                        <th>Comments</th>
                    </tr>
                  </thead>
                  <tbody></tbody>
                </table>
              </div>
            </div>
        </div>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/d3/4.11.0/d3.js"></script>
        <script src="static/js/data.js"></script>
        <script src="static/js/app.js"></script>
    </body>
    </html>

To perform a search we just need to input the information in the filters, the page will update the table of the left showing the result of our search.

![image](https://user-images.githubusercontent.com/88845919/143971046-725ae531-cb34-46f1-9b3d-283db378917c.png)


## Summary

We used javascript to manipuleta UFO sightings information. Then with the help of HTML, we create a web page to interact with the user and filter the information by date, city, state, country and shape.

Here you can see the webpage desing. We include some description and image to give some design to the page. Probably for the future, the filters could be some drop down lists so the user can´t make a mistake. Or also show some message if the information doesn´t match with the user selection. We can link some articles to the page so the user can look for more information.

![image](https://user-images.githubusercontent.com/88845919/143970565-9c8d5c00-d4ea-440a-b87e-d9dfb9be0a24.png)
