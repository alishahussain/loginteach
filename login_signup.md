---
comments: true
layout: base
title: Login - Signup
---

<div class="login-container">
    <div class="card">
        <h3>Login</h3>
        <div class="Email">
            <input id="signInEmailInput" class="input" placeholder="Email">
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

        fetch(`http://localhost:8085/api/person/post?email=${fetchEmail}&password=${fetchPassword}@123&name=${fetchName}`, requestOptions)
        .then(response => {
            if (!response.ok) {
                const errorMsg = 'Login error: ' + response.status;
                console.log(errorMsg);
                return;
            }
            // Success!!!
            // Redirect to Database location
        });
    }

    function login_user() {
        var myHeaders = new Headers();
        myHeaders.append("Content-Type", "application/json");

        var raw = JSON.stringify({
            //"email": document.getElementById("signInEmailInput").value,
            //"password": document.getElementById("signInPasswordInput").value
            "email": "toby@gmail.com",
            "password": "123Toby!"
        });
        console.log(raw);

        var requestOptions = {
            method: 'POST',
            headers: myHeaders,
            credentials: 'include',  // Include this line for cross-origin requests with credentials
            body: raw,
            redirect: 'follow'
        };

        fetch("http://localhost:8085/authenticate", requestOptions)
        .then(response => {
            if (!response.ok) {
                const errorMsg = 'Login error: ' + response.status;
                console.log(errorMsg);

                switch (response.status) {
                    case 401:
                        alert("Incorrect username or password");
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
            return response.text()
        })
        .then(result => {
            console.log(result);
            //window.location.href = "http://127.0.0.1:4100/Login-Lesson/database";
        })
        .catch(error => console.error('Error during login:', error));

    }

</script>