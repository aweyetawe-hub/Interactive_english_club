# Interactive_english_club
learn.speak.grow
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Language Club — Students & Admin</title>
<style>
  :root{
    --blue:#003366;
    --blue-light:#0055aa;
    --accent:#00a3d7;
    --bg:#e6f4ff;
    --card:#ffffff;
  }
  *{box-sizing:border-box}
  body{
    margin:0;
    font-family:Inter, "Segoe UI", Roboto, Arial, sans-serif;
    background:var(--bg);
    color:var(--blue);
    -webkit-font-smoothing:antialiased;
  }
  header{
    background:var(--blue);
    color:#fff;
    display:flex;
    align-items:center;
    justify-content:space-between;
    gap:12px;
    padding:14px 18px;
  }
  header .brand{display:flex;align-items:center;gap:12px}
  header img{height:54px;width:54px;border-radius:8px;object-fit:cover;border:2px solid rgba(255,255,255,0.15)}
  header h1{font-size:20px;margin:0}
  header nav{display:flex;gap:8px;align-items:center}
  header button{
    background:#fff;color:var(--blue);border:none;padding:8px 12px;border-radius:8px;cursor:pointer;font-weight:600;
  }
  header button:hover{background:#f0f8ff}
  main{max-width:1000px;margin:22px auto;padding:0 16px}
  .grid{display:grid;grid-template-columns:repeat(2,1fr);gap:18px}
  .card{background:var(--card);padding:16px;border-radius:12px;box-shadow:0 6px 20px rgba(0,0,0,0.06)}
  h2{margin:0 0 12px 0;color:var(--blue)}
  label{display:block;margin:8px 0 4px;font-size:14px;color:#294861}
  input[type="text"],input[type="email"],input[type="password"],textarea{
    width:100%;padding:10px;border-radius:8px;border:1px solid #cbd8e6;background:#fbfdff;
  }
  textarea{min-height:90px;resize:vertical}
  .btn{background:var(--blue);color:#fff;padding:10px 14px;border-radius:8px;border:none;cursor:pointer;font-weight:600}
  .btn.secondary{background:var(--accent)}
  .btn.danger{background:#c23a3a}
  .btn:disabled{opacity:.6;cursor:not-allowed}
  .topics-list{display:flex;flex-direction:column;gap:12px}
  .topic{border:1px solid #d7eaf7;padding:12px;border-radius:8px;background:#fff}
  .topic h3{margin:0 0 8px 0}
  .meta{font-size:13px;color:#4b6b82;margin-bottom:8px}
  .row{display:flex;gap:8px;flex-wrap:wrap}
  .small{font-size:13px;color:#567}
  footer{max-width:1000px;margin:18px auto;padding:8px 16px;text-align:center;color:#345}
  .hidden{display:none}
  /* responsive */
  @media (max-width:880px){ .grid{grid-template-columns:1fr} header h1{display:none} header img{height:44px;width:44px} }
</style>
</head>
<body>
<header>
  <div class="brand">
    <img id="logo" src="https://via.placeholder.com/80x80.png?text=Logo" alt="Club Logo">
    <div>
      <h1>Language Development Club</h1>
      <div class="small">Join our weekly English practice events</div>
    </div>
  </div>

  <nav>
    <button id="openAuthBtn">Login / Register</button>
    <button id="logoutBtn" class="hidden">Logout</button>
    <button id="helpBtn">Help</button>
  </nav>
</header>

<main>
  <div class="grid">
    <!-- AUTH CARD -->
    <section class="card" id="authCard">
      <h2>Login or Register</h2>
      <div id="authForm">
        <label for="email">Email</label>
        <input id="email" type="email" placeholder="you@example.com" />
        <label for="password">Password</label>
        <input id="password" type="password" placeholder="Enter password (any for demo)" />
        <div style="height:10px"></div>
        <div class="row">
          <button class="btn" id="loginBtn">Login</button>
          <button class="btn secondary" id="registerBtn">Register</button>
        </div>
        <p id="authNotice" class="small" style="margin-top:10px;color:#345">Admin email: <b>aweyetawe@gmail.com</b></p>
      </div>

      <div id="welcomeBox" class="hidden">
        <h2 id="welcomeTitle">Welcome!</h2>
        <p id="welcomeText" class="small">You are logged in.</p>
      </div>
    </section>

    <!-- STUDENT PANEL -->
    <section class="card" id="studentCard">
      <h2>Available Weekly Topics</h2>
      <div id="topicsContainer" class="topics-list">
        <!-- topics appended here -->
      </div>
      <div id="no-topics" class="small">No topics yet. Check back later.</div>
    </section>

    <!-- ADMIN PANEL -->
    <section class="card hidden" id="adminCard">
      <h2>Admin Panel</h2>

      <label>Topic Title</label>
      <input id="topicTitle" type="text" placeholder="e.g., Speaking Club: Daily Conversations" />
      <label>Topic Description</label>
      <textarea id="topicDesc" placeholder="Write what students will do, goals, level..."></textarea>
      <label>Location / Time</label>
      <input id="topicLocation" type="text" placeholder="e.g., School library, Thu 15:00" />

      <label>Upload Logo (optional)</label>
      <input id="logoFile" type="file" accept="image/*" />

      <div style="height:8px"></div>
      <div class="row">
        <button class="btn" id="addTopicBtn">Add Topic</button>
        <button class="btn danger" id="clearTopicsBtn">Clear Topics</button>
      </div>

      <hr style="margin:12px 0">

      <h3>Registered Users</h3>
      <div id="usersList" class="small">No users yet</div>
    </section>

    <!-- PARTICIPANTS / INFO CARD -->
    <section class="card" id="infoCard">
      <h2>My Participation</h2>
      <div id="myJoined" class="small">You are not joined in any event yet.</div>

      <h3 style="margin-top:14px">How to get help</h3>
      <p class="small">Click Help to send an email to admin.</p>
    </section>
  </div>
</main>

<footer>
  © Language Development Club — built for demo. Data stored in your browser (localStorage).
</footer>

<script>
/* ==========================
   Keys in localStorage:
   - ldc_topics  -> JSON array of topics {title,desc,location,createdAt, id}
   - ldc_users   -> JSON array of users {email,name,joined: [topicId,...]}
   - ldc_current -> currentUser {email, role: "admin"|"user", name}
   - ldc_logo    -> dataURL of uploaded logo
   ========================== */

const adminEmail = "aweyetawe@gmail.com";

/* Elements */
const authCard = document.getElementById('authCard');
const adminCard = document.getElementById('adminCard');
const studentCard = document.getElementById('studentCard');
const infoCard = document.getElementById('infoCard');

const openAuthBtn = document.getElementById('openAuthBtn');
const logoutBtn = document.getElementById('logoutBtn');
const helpBtn = document.getElementById('helpBtn');

const emailInput = document.getElementById('email');
const passwordInput = document.getElementById('password');
const loginBtn = document.getElementById('loginBtn');
const registerBtn = document.getElementById('registerBtn');

const topicTitle = document.getElementById('topicTitle');
const topicDesc = document.getElementById('topicDesc');
const topicLocation = document.getElementById('topicLocation');
const logoFile = document.getElementById('logoFile');
const addTopicBtn = document.getElementById('addTopicBtn');
const clearTopicsBtn = document.getElementById('clearTopicsBtn');

const topicsContainer = document.getElementById('topicsContainer');
const noTopics = document.getElementById('no-topics');

const usersList = document.getElementById('usersList');
const myJoined = document.getElementById('myJoined');

const logoImg = document.getElementById('logo');
const welcomeBox = document.getElementById('welcomeBox');
const welcomeTitle = document.getElementById('welcomeTitle');
const welcomeText = document.getElementById('welcomeText');

/* State */
let topics = [];
let users = [];
let current = null;

/* Helpers for storage */
function saveTopics(){ localStorage.setItem('ldc_topics', JSON.stringify(topics)); }
function saveUsers(){ localStorage.setItem('ldc_users', JSON.stringify(users)); }
function saveCurrent(){ localStorage.setItem('ldc_current', JSON.stringify(current)); }
function saveLogo(dataUrl){ localStorage.setItem('ldc_logo', dataUrl); }

/* Load from storage */
function loadAll(){
  const t = localStorage.getItem('ldc_topics');
  topics = t ? JSON.parse(t) : [];
  const u = localStorage.getItem('ldc_users');
  users = u ? JSON.parse(u) : [];
  const c = localStorage.getItem('ldc_current');
  current = c ? JSON.parse(c) : null;
  const lg = localStorage.getItem('ldc_logo');
  if(lg) logoImg.src = lg;
}
loadAll();

/* Utility: generate id */
function genId(){ return 't_' + Date.now() + '_' + Math.floor(Math.random()*9000+1000); }

/* UI updates */
function renderTopicsList(){
  topicsContainer.innerHTML = '';
  if(topics.length === 0){
    noTopics.style.display = 'block';
    return;
  }
  noTopics.style.display = 'none';
  topics.slice().reverse().forEach(t => { // show newest first
    const div = document.createElement('div');
    div.className = 'topic';
    div.innerHTML = `<h3>${escapeHtml(t.title)}</h3>
      <div class="meta">${escapeHtml(t.location)} • ${new Date(t.createdAt).toLocaleString()}</div>
      <p>${escapeHtml(t.desc)}</p>
      <div class="row">
        <button class="btn" onclick="joinTopic('${t.id}')">I want to join</button>
        <button style="background:#fff;color:var(--blue);border:1px solid #d7eaf7;padding:8px;border-radius:6px;cursor:pointer" onclick="viewParticipants('${t.id}')">View participants</button>
      </div>`;
    topicsContainer.appendChild(div);
  });
}

function renderUsersList(){
  if(users.length === 0){ usersList.textContent = 'No registered users'; return; }
  usersList.innerHTML = users.map(u => <div>${escapeHtml(u.name || '(no name)')} — ${escapeHtml(u.email)}</div>).join('');
}

function renderMyJoined(){
  if(!current || current.role !== 'user'){ myJoined.innerHTML = 'Login as a student to see joined events.'; return; }
  const me = users.find(u => u.email === current.email);
  if(!me || !me.joined || me.joined.length === 0){ myJoined.innerHTML = 'You are not joined in any event yet.'; return; }
  const list = me.joined.map(id => {
    const t = topics.find(x=>x.id===id);
    return t ? <div>${escapeHtml(t.title)} — <span class="small">${escapeHtml(t.location)}</span></div> : '';
  }).join('');
  myJoined.innerHTML = list || 'You are not joined in any event yet.';
}

/* Escape helper */
function escapeHtml(s){ if(!s) return ''; return String(s).replace(/[&<>"']/g, function(m){return {'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[m];}); }

/* =======================
   Auth handlers
   ======================= */
function showAuth(){
  authCard.style.display = 'block';
  adminCard.classList.add('hidden');
  studentCard.classList.remove('hidden');
  welcomeBox.classList.add('hidden');
  openAuthBtn.textContent = 'Login / Register';
}
function showAdmin(){
  authCard.classList.add('hidden');
  adminCard.classList.remove('hidden');
  studentCard.classList.remove('hidden');
  welcomeBox.classList.remove('hidden');
  welcomeTitle.textContent = 'Welcome, Admin';
  welcomeText.textContent = 'You are in admin mode.';
  openAuthBtn.textContent = 'Admin';
}
function showStudent(){
  authCard.classList.add('hidden');
  adminCard.classList.add('hidden');
  studentCard.classList.remove('hidden');
  welcomeBox.classList.remove('hidden');
  welcomeTitle.textContent = 'Welcome';
  welcomeText.textContent = Logged in as ${escapeHtml(current.email)};
  openAuthBtn.textContent = current.email;
}

/* Login */
loginBtn.addEventListener('click', () => {
  const email = (emailInput.value || '').trim().toLowerCase();
  const pwd = (passwordInput.value || '').trim();
  if(!email || !pwd){ alert('Enter email & password'); return; }
  if(email === adminEmail){
    // admin login
    current = { email: adminEmail, role: 'admin', name: 'Admin' };
    saveCurrent();
    loginStateChanged();
    return;
  }
  // regular user: find or ask to register
  const u = users.find(x=>x.email === email);
  if(!u){ alert('Email not registered. Please register first or use Register button.'); return; }
  current = { email: u.email, role: 'user', name: u.name || '' };
  saveCurrent();
  loginStateChanged();
});

/* Register */
registerBtn.addEventListener('click', () => {
  const email = (emailInput.value || '').trim().toLowerCase();
  const pwd = (passwordInput.value || '').trim();
  if(!email || !pwd){ alert('Enter email & password'); return; }
  if(email === adminEmail){ alert('This email reserved for admin.'); return; }
  // ask for name
  const name = prompt('Enter your full name (for display):') || '';
  if(users.some(u=>u.email === email)){ alert('Email already registered — you can login.'); return; }
  const newUser = { email, name, joined: [] };
  users.push(newUser);
  saveUsers();
  alert('Registered! Now you can Login.');
});

/* Logout */
document.getElementById('logoutBtn').addEventListener('click', () => {
  current = null;
  localStorage.removeItem('ldc_current');
  loginStateChanged();
});

/* Login-state UI updates */
function loginStateChanged(){
  loadAll(); // refresh arrays from storage
  const c = localStorage.getItem('ldc_current');
  current = c ? JSON.parse(c) : current;
  if(current){
    logoutBtn.classList.remove('hidden');
    openAuthBtn.classList.add('hidden');
    if(current.role === 'admin'){ showAdmin(); }
    else { showStudent(); }
  } else {
    logoutBtn.classList.add('hidden');
    openAuthBtn.classList.remove('hidden');
    showAuth();
  }
  renderTopicsList();
  renderUsersList();
  renderMyJoined();
}

/* On open auth (button) */
openAuthBtn.addEventListener('click', () => {
  // toggle simple behavior: show auth card at top
  authCard.scrollIntoView({behavior:'smooth'});
});

/* =======================
   Topics: add / join / view participants
   ======================= */
addTopicBtn.addEventListener('click', () => {
  if(!current || current.role !== 'admin'){ alert('Only admin can add topics.'); return; }
  const title = (topicTitle.value || '').trim();
  const desc = (topicDesc.value || '').trim();
  const loc = (topicLocation.value || '').trim();
  if(!title || !desc || !loc){ alert('Fill all fields'); return; }
  const id = genId();
  topics.push({ id, title, desc, location: loc, createdAt: Date.now() });
  saveTopics();
  topicTitle.value = ''; topicDesc.value = ''; topicLocation.value = '';
  // handle logo upload if any
  const file = logoFile.files[0];
  if(file){ const reader = new FileReader(); reader.onload = function(e){ logoImg.src = e.target.result; saveLogo(e.target.result);} ; reader.readAsDataURL(file); }
  renderTopicsList();
  renderUsersList();
  alert('Topic added.');
});

/* Clear topics */
clearTopicsBtn.addEventListener('click', () => {
  if(!confirm('Delete ALL topics?')) return;
  topics = [];
  saveTopics();
  renderTopicsList();
});

/* Join topic */
function joinTopic(topicId){
  if(!current || current.role !== 'user'){ alert('Please login as a student to join.'); return; }
  // find user record
  let u = users.find(x=>x.email === current.email);
  if(!u){ alert('User not found (please register/login)'); return; }
  if(!u.joined) u.joined = [];
  if(u.joined.includes(topicId)){ alert('You already joined this event.'); return; }
  u.joined.push(topicId);
  saveUsers();
  renderMyJoined();
  alert('You have joined the event.');
}

/* View participants (admin only) */
function viewParticipants(topicId){
  if(!current || current.role !== 'admin'){ alert('Only admin can view participants.'); return; }
  const participants = users.filter(u=>u.joined && u.joined.includes(topicId));
  if(participants.length === 0) alert('No participants yet.');
  else{
    const list = participants.map(p => ${p.name || '(no name)'} — ${p.email}).join('\n');
    alert('Participants:\n\n' + list);
  }
}

/* =======================
   Users: display list in admin panel
   ======================= */
function updateUsersListUI(){ renderUsersList(); }

/* Help button (mailto to admin) */
helpBtn.addEventListener('click', () => {
  window.location.href = 'mailto:' + encodeURIComponent(adminEmail) + '?subject=Language Club Help';
});

/* Logo upload via file input (separate quick change for admin) */
logoFile.addEventListener('change', (e) => {
  // do nothing here; it's processed when adding topic or you can use immediately:
  const f = e.target.files[0];
  if(!f) return;
  const reader = new FileReader();
  reader.onload = function(ev){
    logoImg.src = ev.target.result;
    saveLogo(ev.target.result);
  };
  reader.readAsDataURL(f);
});

/* Initial render */
loginStateChanged();

/* Export small helpers to global for onclick inline functions */
window.joinTopic = joinTopic;
window.viewParticipants = viewParticipants;

</script>
</body>
</html>
