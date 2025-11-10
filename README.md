# Interactive_english_club
learn.speak.grow
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Language Club</title>
<style>
body {
    font-family: Arial, sans-serif;
    text-align: center;
    background: #e6f0ff; /* жеңіл көк фон */
    margin: 0;
    padding: 0;
}
header {
    background: #004aad; /* қою көк */
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
    background-color: #004aad; /* қою көк кнопка */
    color: white;
}
button:hover { opacity: 0.8; }
.container { margin-top: 50px; }
.card {
    background: white; /* ақ фон */
    padding: 20px;
    margin: 20px auto;
    width: 350px;
    border-radius: 10px;
    box-shadow: 0 0 10px rgba(0,0,0,0.2);
}
.hidden { display: none; }
img { width: 100px; margin-top: 10px; }
input, textarea { width: 90%; padding: 8px; margin: 5px 0; border-radius: 5px; border: 1px solid #ccc; }
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
        <button id="registerBtn">Register</button>
        <button onclick="goBack()">Back</button>
    </div>
</div>

<!-- Login -->
<div class="container hidden" id="loginContainer">
    <div class="card">
        <h3>Login</h3>
        <input type="email" id="loginEmail" placeholder="Email"><br>
        <button id="loginBtn">Login</button>
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
    <div class="card">
        <h3>Admin Panel</h3>
        <input type="text" id="newTopic" placeholder="New Topic"><br>
        <textarea id="newDesc" placeholder="Topic Description"></textarea><br>
        <input type="text" id="newLogo" placeholder="Logo URL"><br>
        <button onclick="updateTopic()">Update Topic</button><br>
        <button onclick="logout()">Logout</button>
    </div>
</div>

<script>
const registerBtn = document.getElementById('registerBtn');
const loginBtn = document.getElementById('loginBtn');

function showRegister(){
    document.getElementById('mainContainer').classList.add('hidden');
    document.getElementById('registerContainer').classList.remove('hidden');
}
function showLogin(){
    document.getElementById('mainContainer').classList.add('hidden');
    document.getElementById('loginContainer').classList.remove('hidden');
}
function goBack(){
    document.getElementById('registerContainer').classList.add('hidden');
    document.getElementById('loginContainer').classList.add('hidden');
    document.getElementById('mainContainer').classList.remove('hidden');
}

// Register
registerBtn.addEventListener('click', ()=>{
    const name = document.getElementById('name').value.trim();
    const email = document.getElementById('email').value.trim();
    if(!name || !email){ alert('Please fill all fields'); return;}
    let users = JSON.parse(localStorage.getItem('users') || '[]');
    if(users.find(u=>u.email===email)){ alert('This email is already registered'); return;}
    users.push({name,email});
    localStorage.setItem('users', JSON.stringify(users));
    alert('Registered successfully!');
    goBack();
});

// Login
loginBtn.addEventListener('click', ()=>{
    const email = document.getElementById('loginEmail').value.trim();
    if(email==='aweyetawe@gmail.com'){
        document.getElementById('loginContainer').classList.add('hidden');
        document.getElementById('adminContainer').classList.remove('hidden');
    } else {
        let users = JSON.parse(localStorage.getItem('users') || '[]');
        let user = users.find(u=>u.email===email);
        if(user){
            document.getElementById('loginContainer').classList.add('hidden');
            document.getElementById('studentContainer').classList.remove('hidden');
            document.getElementById('studentName').innerText = user.name;
        } else {
            alert('Email not registered!');
        }
    }
});

function logout(){
    document.getElementById('adminContainer').classList.add('hidden');
    document.getElementById('studentContainer').classList.add('hidden');
    document.getElementById('mainContainer').classList.remove('hidden');
}
function sendHelp(){
    window.location.href='mailto:aweyetawe@gmail.com?subject=Help';
}
function joinActivity(){
    alert('You have joined this activity!');
}
function updateTopic(){
    const topic = document.getElementById('newTopic').value.trim();
    const desc = document.getElementById('newDesc').value.trim();
    const logoURL = document.getElementById('newLogo').value.trim();
    if(topic){ 
        localStorage.setItem('weeklyTopic', topic);
        document.getElementById('weeklyTopic').innerText = topic;
    }
    if(desc){ 
        localStorage.setItem('topicDesc', desc);
        document.getElementById('topicDescription').innerText = desc;
    }
    if(logoURL){
        localStorage.setItem('logoURL', logoURL);
        document.getElementById('logo').src = logoURL;
    }
    alert('Topic updated successfully!');
}

window.onload = function(){
    const topic = localStorage.getItem('weeklyTopic');
    if(topic) document.getElementById('weeklyTopic').innerText = topic;
    const desc = localStorage.getItem('topicDesc');
    if(desc) document.getElementById('topicDescription').innerText = desc;
    const logoURL = localStorage.getItem('logoURL');
    if(logoURL) document.getElementById('logo').src = logoURL;
}
</script>

</body>
</html>
