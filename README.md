# Interactive_english_club
learn.speak.grow
<!DOCTYPE html>
<html lang="kk">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Клубқа тіркелу</title>
<style>
body { font-family: Arial, sans-serif; margin: 20px; background: #f0f8ff; }
h1, h2 { color: #333; }
form, .admin-panel, .topic-section { background: #fff; padding: 20px; border-radius: 10px; margin-bottom: 20px; box-shadow: 0 0 10px rgba(0,0,0,0.1);}
input, button, select { padding: 10px; margin: 5px 0; width: 100%; box-sizing: border-box; border-radius: 5px; border: 1px solid #ccc;}
button { cursor: pointer; background-color: #4CAF50; color: white; border: none;}
button:hover { background-color: #45a049; }
.hidden { display: none; }
</style>
</head>
<body>

<h1>Клубқа тіркелу</h1>

<div id="register-section">
<form id="register-form">
  <label>Аты-жөніңіз:</label>
  <input type="text" id="name" required>
  <label>Email:</label>
  <input type="email" id="email" required>
  <button type="submit">Тіркелу</button>
</form>
</div>

<div id="topic-section" class="topic-section hidden">
<h2>Апталық тақырып</h2>
<p id="topic-text"></p>
<p id="topic-place"></p>
<button onclick="help()">Көмек</button>
</div>

<div id="admin-section" class="admin-panel hidden">
<h2>Админ панелі</h2>
<label>Апталық тақырыпты өзгерту:</label>
<input type="text" id="admin-topic">
<label>Орын/уақыты:</label>
<input type="text" id="admin-place">
<button onclick="updateTopic()">Сақтау</button>
</div>

<script>
// Жүйенің логикасы
let currentUser = null;

// Алғашқы тақырып
if(!localStorage.getItem('weeklyTopic')) {
  localStorage.setItem('weeklyTopic', 'Тілді дамытуға арналған ішкі шара');
  localStorage.setItem('weeklyPlace', 'Клуб бөлмесі, 14:00');
}

// Тіркелу
document.getElementById('register-form').addEventListener('submit', function(e){
  e.preventDefault();
  const name = document.getElementById('name').value;
  const email = document.getElementById('email').value;
  currentUser = {name, email};
  localStorage.setItem('currentUser', JSON.stringify(currentUser));
  
  // Егер админ болса, админ панелін көрсету
  if(email === 'admin@example.com'){
    document.getElementById('admin-section').classList.remove('hidden');
  }
  
  document.getElementById('register-section').classList.add('hidden');
  document.getElementById('topic-section').classList.remove('hidden');
  showTopic();
});

// Апталық тақырыпты көрсету
function showTopic(){
  document.getElementById('topic-text').innerText = localStorage.getItem('weeklyTopic');
  document.getElementById('topic-place').innerText = 'Орны/уақыты: ' + localStorage.getItem('weeklyPlace');
}

// Көмек батырмасы
function help(){
  window.location.href = 'mailto:youremail@example.com?subject=Клубқа көмек';
}

// Админ тақырыпты өзгерту
function updateTopic(){
  const newTopic = document.getElementById('admin-topic').value;
  const newPlac
