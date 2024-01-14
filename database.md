---
layout: base
title: User Database
description: cool
permalink: /database
---

## SQL Database

<div id="userData"></div>

<!-- HTML table fragment for page -->
<table>
  <thead>
  <tr>
    <th>Name</th>
    <th>ID</th>
    <th>Age</th>
  </tr>
  </thead>
  <tbody id="result">
    <!-- javascript generated data -->
  </tbody>
</table>

<!-- Script is layed out in a sequence (no function) and will execute when page is loaded -->
<script>

  function userDbRequest() {

    // prepare HTML result container for new output
    const resultContainer = document.getElementById("result");

    // set options for cross origin header request
    const options = {
      method: 'GET', // *GET, POST, PUT, DELETE, etc.
      mode: 'cors', // no-cors, *cors, same-origin
      cache: 'default', // *default, no-cache, reload, force-cache, only-if-cached
      credentials: 'include', // include, *same-origin, omit
      headers: {
        'Content-Type': 'application/json',
      },
    };

    // fetch the API
    fetch("http://localhost:8085/api/person/", options)
      // response is a RESTful "promise" on any successful fetch
      .then(response => {
        // check for response errors and display
        if (response.status !== 200) {
            const errorMsg = 'Database response error: ' + response.status;
            console.log(errorMsg);
            const tr = document.createElement("tr");
            const td = document.createElement("td");
            td.innerHTML = errorMsg;
            tr.appendChild(td);
            resultContainer.appendChild(tr);
            return;
        }
        // valid response will contain json data
        response.json().then(data => {
            console.log(data);
            for (const row of data) {
              // tr and td build out for each row
              const tr = document.createElement("tr");
              const name = document.createElement("td");
              const id = document.createElement("td");
              const age = document.createElement("td");
              // data is specific to the API
              name.innerHTML = row.name;
              id.innerHTML = row.email;
              age.innerHTML = row.age;
              // this build td's into tr
              tr.appendChild(name);
              tr.appendChild(id);
              tr.appendChild(age);
              // add HTML to container
              resultContainer.appendChild(tr);
            }
        })
    })
    // catch fetch errors (ie ACCESS to server blocked)
    .catch(err => {
      console.error(err);
      const tr = document.createElement("tr");
      const td = document.createElement("td");
      td.innerHTML = err + ": " + url;
      tr.appendChild(td);
      resultContainer.appendChild(tr);
    });
  }

  function fetchUserData() {
      var requestOptions = {
        method: 'GET',
        mode: 'cors',
        cache: 'default',
        credentials: 'include',
      };

      fetch("http://localhost:8085/api/person/jwt", requestOptions)
        .then(response => response.json())
        .then(data => {
          // Display user data above the table
          const userDataContainer = document.getElementById("userData");
          userDataContainer.innerHTML = `
            <h1><strong>${data.name}</strong></h1>
            <p>Email: ${data.email}</p>
            <p>Age: ${data.age}</p>
            <p>ID: ${data.id}</p>
          `;
          console.log(data);
        })
        .catch(error => console.log('error', error));
  }

  window.onload = function() {
    fetchUserData();
    userDbRequest();
  }
</script>

<style>
  #userData {
    text-align: center;
  }
</style>