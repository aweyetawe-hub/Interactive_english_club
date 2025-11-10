# Interactive_english_club
learn.speak.grow
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Language Club</title>
<style>
body {
    font-family: Arial, sans-serif;
    text-align: center;
    background: #f0f0f0;
    margin: 0;
    padding: 0;
}
header {
    background: #4CAF50;
    color: white;
    padding: 20px;
    font-size: 24px;
}
button {
    padding: 10px 20px;
    margin: 10px;
    font-size: 16px;
    border-radius: 8px;
    cursor: pointer;
    border: none;
    transition: 0.3s;
}
button:hover { opacity: 0.8; }
.container { margin-top: 50px; }
.card {
    background: white;
    padding: 20px;
    margin: 20px auto;
    width: 350px;
    border-radius: 10px;
    box-shadow: 0 0 10px rgba(0,0,0,0.2);
}
.hidden { display: none; }
img { width: 100px; margin-top: 10px; }
input, textarea { width: 90%; padding: 8px; margin: 5px 0; }
</style>
</head>
<body>

<header>
    <img id="logo" src="https://via.placeholder.com/100" alt="Logo">
    <div>Language Development Club</div>
</header>

<div class="container" id="mainContainer">
    <button onclick="showRegister()">Register</button>
    <button onclick="showLogin()">Login</button>
</div>

<!-- Register -->
<div class="container hidden" id="registerContainer">
    <div class="card">
        <h3>Register</h3>
        <input type="text" id="name" placeholder="Your Name"><br>
        <input type="email" id="email" placeholder="Email"><br>
        <button onclick="registerUser()">Register</button>
        <button onclick="goBack()">Back</button>
    </div>
</div>

<!-- Login -->
<div class="container hidden" id="loginContainer">
    <div class="card">
        <h3>Login</h3>
        <input type="email" id="loginEmail" placeholder="Email"><br>
        <button onclick="loginUser()">Login</button>
        <button onclick="goBack()">Back</button>
    </div>
</div>

<!-- Student Page -->
<div class="container hidden" id="studentContainer">
    <div class="card">
        <h3>Welcome, <span id="studentName"></span>!</h3>
        <p>Weekly Topic: <strong id="weeklyTopic">Language Improvement Activity</strong></p>
        <p id="topicDescription">Join to improve your English skills.</p>
        <button onclick="joinActivity()">I want to join</button><br>
        <button onclick="sendHelp()">Help</button><br>
        <button onclick="logout()">Logout</button>
    </div>
</div>

<!-- Admin Page -->
<div class="container hidden" id="adminContainer">
