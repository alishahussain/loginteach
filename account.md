---
layout: base
title: Account
description: account page with user data
permalink: /account
---

<div id="userData">
</div>

<!-- HTML table fragment for page -->
<table>
  <thead>
  <tr>
    <th>Name</th>
    <th>Email</th>
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
        .then(response => {
                if (!response.ok) {
                    const errorMsg = 'Login error: ' + response.status;
                    console.log(errorMsg);

                    switch (response.status) {
                        case 401:
                            alert("Please log into or make an account");
                            window.location.href = "http://127.0.0.1:4100/loginteach/loginSignup";
                            break;
                        case 403:
                            alert("Access forbidden. You do not have permission to access this resource.");
                            break;
                        case 404:
                            alert("User not found. Please check your credentials.");
                            break;
                        // Add more cases for other status codes as needed
                        default:
                            alert("Login failed. Please try again later.");
                    }

                    return Promise.reject('Login failed');
                }
                return response.json();
                // Success!!!
            })
        .then(data => {
          // Display user data above the table
          const userDataContainer = document.getElementById("userData");
          userDataContainer.innerHTML = `
            <img src="/loginteach/images/defaultUser.png" width="250" height="250">
            <h1><strong>${data.name}</strong></h1>
            <p>Email: ${data.email}</p>
            <p>Age: ${data.age}</p>
            <p>ID: ${data.id}</p>
            <button onclick="signOut()">Sign Out</button>
          `;
          console.log(data);
        })
        .catch(error => console.log('error', error));
  }

  function clearCookie(name, domain, path) {
    // Set the expiration date to a past date
    document.cookie = `${name}=; expires=Thu, 01 Jan 1970 00:00:00 UTC; domain=${domain}; path=${path}; HttpOnly=${true}; Secure=${true}; SameSite=None`;
  }

// Call the function with your cookie details

  function signOut() {
    console.log("signout called");
    clearCookie('jwt', 'localhost', '/');
  }

  window.onload = function() {
    fetchUserData();
    userDbRequest();
  }
</script>

<style>
  #userData {
    text-align: center;
    background-color: #242222;
    color: white;
    padding: 20px;
    border-radius: 50px 50px 0px 0px;
  }
</style>