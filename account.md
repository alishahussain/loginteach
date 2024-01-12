---
layout: base
title: Account
description: account page with user data
permalink: /account
---

<body onload="windowLoad();">
    
</body>

<script>
    function windowLoad() {
        var requestOptions = {
            method: 'GET',
            redirect: 'follow'
        };

    fetch("http://localhost:8085/api/person/jwt", requestOptions)
    .then(response => {
            if (!response.ok) {
                const errorMsg = 'Login error: ' + response.status;
                console.log(errorMsg);

                switch (response.status) {
                    case 401:
                        alert("Please log into or make an account");
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

            // Success!!!
        })
    .then(result => console.log(result))
    .catch(error => console.log('error', error));
    }
</script>