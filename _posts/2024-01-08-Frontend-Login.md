---
comments: true
layout: notebook
title: Wireframes
description: CSS and HTML behiend a good login
author: Finn Carpenter
---

<div class="login-container">
    <div class="card">
        <h3>Login</h3>
        <div class="Name">
            <input id="signInNameInput" class="input" placeholder="Name">
        </div>
        <div class="Password">
            <input id="signInPasswordInput" class="input" placeholder="Password">
        </div>
        <div class="Buttons">
            <button class="signInButton" onclick="login_user()">Login</button>
        </div>
    </div>
</div>

<br>

<div class="login-container">
    <div class="card2">
        <h3>Sign-Up</h3>
        <div class="Name">
            <input id="signUpNameInput" class="input" placeholder="Name">
        </div>
        <div class="Email">
            <input id="signUpEmailInput" class="input" placeholder="Email">
        </div>
        <div class="Password">
            <input id="signUpPasswordInput" type="password" class="input" placeholder="Password">
        </div>
        <div class="Buttons">
            <button class="signUpButton" onclick="signup_user()">Sign Up</button>
        </div>
    </div>
</div>


<script>
    function login_user() {

                // You can make a POST request here to your authentication endpoint
                var url = "";

                // Comment out next line for local testing
                // url = "http://localhost:8085";
                const login_url = url + '/authenticate';
                const body = {
                    email: document.getElementById("signInNameInput").value,
                    password: document.getElementById("signInPasswordInput").value,
                };

                console.log(JSON.stringify(body));
                const requestOptions = {
                    method: 'POST',
                    mode: 'cors',
                    cache: 'no-cache',
                    credentials: 'include',
                    body: JSON.stringify(body),
                    headers: {
                        "content-type": "application/json",
                        "Access-Control-Allow-Credentials": "true",
                        "Access-Control-Allow-Origin": "*",
                    },
                };

                // Fetch JWT
                fetch(login_url, requestOptions)
                    .then(response => {
                        if (!response.ok) {
                            const errorMsg = 'Login error: ' + response.status;
                            console.log(errorMsg);
                            return;
                        }
                        // Success!!!
                        // code for success like redirect
                });
    }

    function signup_user() {
        var requestOptions = {
            method: 'POST',
            mode: 'cors',
            cache: 'no-cache',
            credentials: 'include',
        };

        let fetchName = document.getElementById("signUpNameInput").value
        let fetchEmail = document.getElementById("signUpEmailInput").value
        let fetchPassword = document.getElementById("signUpPasswordInput").value

        fetch(`https://DEPLOYED_URL.com/api/person/post?email=${fetchEmail}&password=${fetchPassword}@123&name=${fetchName}`, requestOptions)
        .then(response => {
            if (!response.ok) {
                const errorMsg = 'Login error: ' + response.status;
                console.log(errorMsg);
                return;
            }
            // Success!!!
            // Redirect to Database location
            console.log("success")
        });
    }
    
</script>