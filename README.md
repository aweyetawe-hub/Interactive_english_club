# Interactive_english_club
learn.speak.grow
<!DOCTYPE html>
<html lang="kk">
<head>
<meta charset="UTF-8">
<title>Клубқа тіркелу</title>
<style>
body {
    font-family: Arial, sans-serif;
    text-align: center;
    background: #f5f5f5;
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
button:hover {
    opacity: 0.8;
}
#registerBtn {
    background: #32cd32;
    color: white;
    font-weight: bold;
    font-size: 18px;
}
#loginBtn {
    background: #008CBA;
    color: white;
    font-weight: bold;
    font-size: 18px;
}
.container {
    margin-top: 50px;
}
.card {
    background: white;
    padding: 20px;
    margin: 20px auto;
    width: 300px;
    border-radius: 10px;
    box-shadow: 0 0 10px rgba(0,0,0,0.2);
}
.hidden {
    display: none;
}
img {
    width: 100px;
    margin-top: 10px;
}
</style>
</head>
<body>

<header>
    <img src="https://via.placeholder.com/100" alt="Эмблема">
    <div>Тілді дамыту клубы</div>
</header>

<div class="container" id="mainContainer">
    <button id="registerBtn" onclick="showRegister()">Тіркелу</button>
    <button id="loginBtn" onclick="showLogin()">Кіру</button>
</div>

<div class="container hidden" id="registerContainer">
    <div class="card">
        <h3>Тіркелу</h3>
        <input type="text" id="name" placeholder="Аты-жөніңіз"><br><br>
        <input type="email" id="email" placeholder="Email"><br><br>
        <button onclick="registerUser()">Тіркелу</button>
        <button onclick="goBack()">Артқа</button>
    </div>
</div>

<div class="container hidden" id="loginContainer">
    <div class="card">
        <h3>Кіру</h3>
        <input type="email" id="loginEmail" placeholder="Email"><br><br>
        <button onclick="loginUser()">Кіру</button>
        <button onclick="goBack()">Артқа</button>
    </div>
</div>

<div class="container hidden" id="studentContainer">
    <div class="card">
        <h3>Қош келдіңіз, <span id="studentName"></span>!</h3>
        <p>Апта тақырыбы: <span id="weeklyTopic">Тілді дамыту мақсатындағы іш шара</span></p>
        <button onclick="alert('Сіз шараға тіркелдіңіз!')">Қатысуға тіркелу</button><br>
        <button onclick="sendHelp()">Көмек</button><br>
        <button onclick="logout()">Шығу</button>
    </div>
</div>

<div class="container hidden" id="adminContainer">
    <div class="card">
        <h3>Админ панелі</h3>
        <input type="text" id="newTopic" placeholder="Жаңа тақырып енгізіңіз"><br><br>
        <button onclick="updateTopic()">Тақырыпты жаңарту</button><br><br>
        <button onclick="logout()">Шығу</button>
    </div>
</div>

<script>
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
function registerUser(){
    const name = document.getElementById('name').value;
    const email = document.getElementById('email').value;
    if(!name || !email){ alert('Барлық өрістерді толтырыңыз!'); return;}
    let users = JSON.parse(localStorage.getItem('users') || '[]');
    if(users.find(u=>u.email===email)){ alert('Бұл email бұрын тіркелген!'); return;}
    users.push({name,email});
    localStorage.setItem('users', JSON.stringify(users));
    alert('Тіркелу сәтті өтті!');
    goBack();
}
function loginUser(){
    const email = document.getElementById('loginEmail').value;
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
            alert('Тіркелмеген email!');
        }
    }
}
function logout(){
    document.getElementById('adminContainer').classList.add('hidden');
    document.getElementById('studentContainer').classList.add('hidden');
    document.getElementById('mainContainer').classList.remove('hidden');
}
function sendHelp(){
    window.location.href='mailto:aweyetawe@gmail.com?subject=Көмек';
}
function updateTopic(){
    const topic = document.getElementById('newTopic').value;
    if(!topic){ alert('Тақырып енгізіңіз'); return;}
    localStorage.setItem('weeklyTopic', topic);
    alert('Тақырып жаңартылды!');
}
window.onload = function(){
    const topic = localStorage.getItem('weeklyTopic');
    if(topic) document.getElementById('weeklyTopic').innerText = topic;
}
</script>

</body>
</html>
