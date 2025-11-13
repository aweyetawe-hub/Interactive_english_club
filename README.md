# Interactive_english_club
learn.speak.grow
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Language Club Portal</title>
<style>
:root{
  --nav:#003366;
  --accent:#0055aa;
  --bg:#e9f4ff;
  --card:#fff;
}
*{box-sizing:border-box}
body{
  margin:0;font-family:Arial,Helvetica,sans-serif;background:var(--bg);color:var(--nav);
}
header{
  background:var(--nav);color:#fff;padding:12px 18px;display:flex;align-items:center;justify-content:space-between;gap:12px;
}
.brand{display:flex;align-items:center;gap:12px}
.brand img{width:56px;height:56px;border-radius:8px;object-fit:cover;border:2px solid rgba(255,255,255,0.15)}
.brand h1{font-size:18px;margin:0}
nav{display:flex;gap:8px;align-items:center}
button{cursor:pointer}
.btn{background:var(--nav);color:#fff;border:none;padding:8px 12px;border-radius:8px;font-weight:600}
.btn.ghost{background:#fff;color:var(--nav);border:2px solid rgba(255,255,255,0.08)}
.container{max-width:1100px;margin:20px auto;padding:0 14px}
.grid{display:grid;grid-template-columns:1fr 420px;gap:16px}
.card{background:var(--card);padding:16px;border-radius:12px;box-shadow:0 8px 20px rgba(3,30,60,0.06)}
.input, textarea{width:100%;padding:10px;border-radius:8px;border:1px solid #d6e8fb;background:#fbfdff;margin:8px 0}
textarea{min-height:96px;resize:vertical}
.topic{border:1px solid #d6e8fb;padding:12px;border-radius:10px;background:#fff;margin-bottom:12px}
.small{font-size:13px;color:#4b6b82}
.hidden{display:none}
.footer{max-width:1100px;margin:18px auto;text-align:center;color:#345}
@media(max-width:900px){.grid{grid-template-columns:1fr}}
</style>
</head>
<body>
<header>
  <div class="brand">
    <img id="logo" src="https://via.placeholder.com/120x120.png?text=Club" alt="Logo">
    <div>
      <h1>Language Club Portal</h1>
      <div class="small">Weekly English practice — join events & improve fluency</div>
    </div>
  </div>

  <nav>
    <button id="openAuth" class="btn ghost">Login / Register</button>
    <button id="logoutBtn" class="btn hidden">Logout</button>
    <button id="helpBtn" class="btn">Help</button>
  </nav>
</header>

<div class="container">
  <div class="grid">
    <!-- LEFT: main area (auth / admin or user topics) -->
    <div>
      <!-- AUTH CARD -->
      <div id="authCard" class="card">
        <h2>Login or Register</h2>
        <label class="small">Name (for students)</label>
        <input id="nameInput" class="input" placeholder="Your full name (students only)">
        <label class="small">Email</label>
        <input id="emailInput" class="input" type="email" placeholder="you@example.com">
        <label class="small">Password</label>
        <input id="passInput" class="input" type="password" placeholder="Choose a password">
        <div style="display:flex;gap:8px;margin-top:8px">
          <button id="registerBtn" class="btn">Register (student)</button>
          <button id="loginBtn" class="btn ghost">Login</button>
        </div>
        <p class="small" style="margin-top:10px;color:#345">Admin email: <b>aweyetawe@gmail.com</b> (use Login for admin)</p>
        <p id="authMsg" class="small" style="color:#b23"></p>
      </div>

      <!-- ADMIN CARD -->
      <div id="adminCard" class="card hidden" style="margin-top:16px">
        <h2>Admin Panel</h2>
        <label class="small">Event Title</label>
        <input id="titleInput" class="input" placeholder="e.g., Speaking Session: Daily Conversation">
        <label class="small">Description</label>
        <textarea id="descInput" class="input" placeholder="Describe the activity, target level, tasks..."></textarea>
        <label class="small">Location / Time</label>
        <input id="locInput" class="input" placeholder="e.g., School library — Fri 15:00">
        <label class="small">Change Logo (optional)</label>
        <input id="logoFile" class="input" type="file" accept="image/*">
        <div style="display:flex;gap:8px;margin-top:8px">
          <button id="addTopicBtn" class="btn">Add Event</button>
          <button id="clearBtn" class="btn ghost">Clear All Events</button>
          <button id="viewUsersBtn" class="btn ghost">View Registered Users</button>
        </div>
        <div id="adminUsers" style="margin-top:12px"></div>
      </div>

      <!-- USER JOIN PAGE -->
      <div id="userCard" class="card hidden" style="margin-top:16px">
        <h2>Available Events</h2>
        <div id="topicsList"></div>
      </div>

      <!-- JOINED EVENTS -->
      <div id="joinedCard" class="card hidden" style="margin-top:16px">
        <h2>My Joined Events</h2>
        <div id="joinedList" class="small">You haven't joined any events yet.</div>
      </div>
    </div>

    <!-- RIGHT: info / quick actions -->
    <aside>
      <div class="card">
        <h3>Quick Info</h3>
        <p class="small">When registered as a student you will only see the user view. Admin panel is hidden from students.</p>
        <p class="small"><b>Help</b> opens an email to admin.</p>
      </div>

      <div class="card" style="margin-top:12px">
        <h3>About</h3>
        <p class="small">This demo stores data in your browser (localStorage). To reset, clear browser storage for this site.</p>
      </div>
    </aside>
  </div>
</div>

<div class="footer">© Language Club — local demo. Data stored in your browser.</div>

<script>
/* --- Storage keys --- */
const KEY_TOPICS = 'ldc_topics_v1';
const KEY_USERS  = 'ldc_users_v1';
const KEY_CURR   = 'ldc_current_v1';
const KEY_LOGO   = 'ldc_logo_v1';

/* --- Admin email --- */
const ADMIN_EMAIL = 'aweyetawe@gmail.com';

/* --- Elements --- */
const openAuth = document.getElementById('openAuth');
const logoutBtn = document.getElementById('logoutBtn');
const helpBtn = document.getElementById('helpBtn');

const authCard = document.getElementById('authCard');
const adminCard = document.getElementById('adminCard');
const userCard = document.getElementById('userCard');
const joinedCard = document.getElementById('joinedCard');

const nameInput = document.getElementById('nameInput');
const emailInput = document.getElementById('emailInput');
const passInput = document.getElementById('passInput');
const registerBtn = document.getElementById('registerBtn');
const loginBtn = document.getElementById('loginBtn');
const authMsg = document.getElementById('authMsg');

const titleInput = document.getElementById('titleInput');
const descInput = document.getElementById('descInput');
const locInput = document.getElementById('locInput');
const logoFile = document.getElementById('logoFile');
const addTopicBtn = document.getElementById('addTopicBtn');
const clearBtn = document.getElementById('clearBtn');
const viewUsersBtn = document.getElementById('viewUsersBtn');

const topicsList = document.getElementById('topicsList');
const joinedList = document.getElementById('joinedList');
const adminUsers = document.getElementById('adminUsers');

const logoImg = document.getElementById('logo');

/* --- State --- */
let topics = JSON.parse(localStorage.getItem(KEY_TOPICS) || '[]');
let users  = JSON.parse(localStorage.getItem(KEY_USERS) || '[]');
let current = JSON.parse(localStorage.getItem(KEY_CURR) || 'null');

/* --- Helpers --- */
function saveTopics(){ localStorage.setItem(KEY_TOPICS, JSON.stringify(topics)); }
function saveUsers(){ localStorage.setItem(KEY_USERS, JSON.stringify(users)); }
function saveCurrent(){ localStorage.setItem(KEY_CURR, JSON.stringify(current)); }
function saveLogoData(url){ localStorage.setItem(KEY_LOGO, url); }

/* escape */
function esc(s){ return String(s||'').replace(/[&<>"']/g, c=>({ '&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;' }[c])); }

/* load persisted logo */
(function loadLogo(){
  const ld = localStorage.getItem(KEY_LOGO);
  if(ld) logoImg.src = ld;
})();

/* --- UI rendering --- */
function renderTopics(){
  topicsList.innerHTML = '';
  if(topics.length === 0){
    topicsList.innerHTML = '<p class="small">No events yet — admin will add events.</p>';
    return;
  }
  topics.slice().reverse().forEach((t)=>{
    const el = document.createElement('div');
    el.className = 'topic';
    el.innerHTML = `<h3>${esc(t.title)}</h3>
      <div class="small">${esc(t.location)} • ${new Date(t.createdAt).toLocaleString()}</div>
      <p>${esc(t.desc)}</p>
      <div style="display:flex;gap:8px;margin-top:6px">
        <button class="btn" onclick="joinEvent('${t.id}')">I want to join</button>
        <button class="btn ghost" onclick="viewParticipants('${t.id}')">View participants</button>
      </div>`;
    topicsList.appendChild(el);
  });
}

function renderJoined(){
  if(!current || current.role !== 'user'){ joinedList.innerHTML = '<p class="small">Login as student to see joined events.</p>'; return; }
  const u = users.find(x=>x.email === current.email);
  if(!u || !u.joined || u.joined.length === 0){ joinedList.innerHTML = '<p class="small">No joined events yet.</p>'; return; }
  joinedList.innerHTML = u.joined.map(id=>{
    const t = topics.find(x=>x.id===id);
    if(!t) return '';
    return `<div class="topic"><strong>${esc(t.title)}</strong><div class="small">${esc(t.location)}</div><p>${esc(t.desc)}</p></div>`;
  }).join('');
}

function renderUsersList(){
  if(users.length === 0){ adminUsers.innerHTML = '<p class="small">No users registered yet.</p>'; return;}
  adminUsers.innerHTML = users.map(u=>`<div class="small">${esc(u.name||'(no name)')} — ${esc(u.email)}</div>`).join('');
}

/* --- Utility id gen --- */
function genId(){ return 'id_'+Date.now()+'_'+Math.floor(Math.random()*9000); }

/* --- Auth logic --- */
registerBtn.addEventListener('click', ()=>{
  const name = nameInput.value.trim();
  const email = emailInput.value.trim().toLowerCase();
  const pass = passInput.value;
  authMsg.textContent = '';
  if(!email || !pass){ authMsg.textContent = 'Please enter email and password.'; return; }
  if(email === ADMIN_EMAIL){ authMsg.textContent = 'Use Login for admin account.'; return; }
  if(users.some(u=>u.email === email)){ authMsg.textContent = 'Email already registered. Please login.'; return; }
  const newUser = { email, password: pass, name: name || '', role: 'user', joined: [] };
  users.push(newUser);
  saveUsers();
  current = { email, role: 'user' };
  saveCurrent();
  // clear inputs
  nameInput.value=''; emailInput.value=''; passInput.value='';
  // show user view
  showUserView();
});

loginBtn.addEventListener('click', ()=>{
  const email = emailInput.value.trim().toLowerCase();
  const pass = passInput.value;
  authMsg.textContent = '';
  if(!email || !pass){ authMsg.textContent = 'Please enter email and password.'; return; }
  if(email === ADMIN_EMAIL){
    // admin login (no stored password required here; any password accepted for admin demo)
    current = { email: ADMIN_EMAIL, role: 'admin' };
    saveCurrent();
    nameInput.value=''; emailInput.value=''; passInput.value='';
    showAdminView();
    return;
  }
  const user = users.find(u=>u.email===email);
  if(!user){ authMsg.textContent = 'Email not registered. Please register first.'; return; }
  if(user.password !== pass){ authMsg.textContent = 'Incorrect password.'; return; }
  current = {email: user.email, role:'user'};
  saveCurrent();
  nameInput.value=''; emailInput.value=''; passInput.value='';
  showUserView();
});

/* Logout */
logoutBtn.addEventListener('click', ()=>{
  current = null;
  localStorage.removeItem(KEY_CURR);
  adminCard.classList.add('hidden');
  userCard.classList.add('hidden');
  joinedCard.classList.add('hidden');
  authCard.classList.remove('hidden');
  logoutBtn.classList.add('hidden');
});

/* Show admin UI */
function showAdminView(){
  authCard.classList.add('hidden');
  adminCard.classList.remove('hidden');
  userCard.classList.remove('hidden'); // admin can still see events preview
  joinedCard.classList.add('hidden');
  logoutBtn.classList.remove('hidden');
  renderTopics();
  renderUsersList();
}

/* Show user UI */
function showUserView(){
  authCard.classList.add('hidden');
  adminCard.classList.add('hidden');
  userCard.classList.remove('hidden');
  joinedCard.classList.remove('hidden');
  logoutBtn.classList.remove('hidden');
  renderTopics();
  renderJoined();
}

/* If current in storage, auto show correct panel */
if(current){
  if(current.role === 'admin') showAdminView();
  else showUserView();
}

/* Add topic (admin) */
addTopicBtn.addEventListener('click', ()=>{
  if(!current || current.role!=='admin'){ alert('Only admin can add events.'); return; }
  const title = titleInput.value.trim();
  const desc = descInput.value.trim();
  const loc = locInput.value.trim();
  if(!title || !desc || !loc){ alert('Please fill all event fields.'); return; }
  const id = genId();
  topics.push({ id, title, desc, location: loc, createdAt: Date.now() });
  saveTopics();
  // logo file if selected
  const f = logoFile.files[0];
  if(f){
    const r = new FileReader();
    r.onload = function(e){ logoImg.src = e.target.result; saveLogoData(e.target.result); };
    r.readAsDataURL(f);
  }
  titleInput.value=''; descInput.value=''; locInput.value=''; logoFile.value='';
  renderTopics();
  alert('Event added.');
});

/* Clear topics (admin) */
clearBtn.addEventListener('click', ()=>{
  if(!current || current.role!=='admin'){ alert('Only admin can clear.'); return; }
  if(!confirm('Delete ALL events?')) return;
  topics = []; saveTopics(); renderTopics(); alert('All events cleared.');
});

/* View registered users (in admin panel) */
viewUsersBtn.addEventListener('click', ()=>{ renderUsersList(); });

/* Join event (student) */
window.joinEvent = function(eventId){
  if(!current || current.role !== 'user'){ alert('Please login as student to join.'); return; }
  const user = users.find(u=>u.email === current.email);
  if(!user){ alert('User record not found. Try logging in again.'); return; }
  if(!user.joined) user.joined = [];
  if(user.joined.includes(eventId)){ alert('You already joined this event.'); return; }
  user.joined.push(eventId);
  saveUsers();
  renderJoined();
  alert('You joined the event.');
};

/* View participants (admin) */
window.viewParticipants = function(eventId){
  if(!current || current.role!=='admin'){ alert('Only admin can view participants.'); return; }
  const parts = users.filter(u=>u.joined && u.joined.includes(eventId));
  if(parts.length === 0) alert('No participants yet.');
  else alert('Participants:\\n\\n' + parts.map(p => (p.name||'(no name)') + ' — ' + p.email).join('\\n'));
};

/* render topics used by admin or student views */
function renderTopics(){
  topicsList.innerHTML = '';
  if(topics.length === 0){ topicsList.innerHTML = '<p class="small">No events yet.</p>'; return; }
  topics.slice().reverse().forEach(t => {
    const d = document.createElement('div'); d.className='topic';
    d.innerHTML = `<h3>${esc(t.title)}</h3><div class="small">${esc(t.location)} • ${new Date(t.createdAt).toLocaleString()}</div>
      <p>${esc(t.desc)}</p>
      <div style="margin-top:6px"><button class="btn" onclick="joinEvent('${t.id}')">I want to join</button>
      <button class="btn ghost" onclick="viewParticipants('${t.id}')">View participants</button></div>`;
    topicsList.appendChild(d);
  });
}

/* renderUsersList helper */
function renderUsersList(){
  if(users.length === 0) adminUsers.innerHTML = '<p class="small">No users registered.</p>';
  else adminUsers.innerHTML = users.map(u => `<div class="small">${esc(u.name||'(no name)')} — ${esc(u.email)}</div>`).join('');
}

/* renderJoined for current user */
function renderJoined(){
  if(!current || current.role !== 'user'){ joinedList.innerHTML = '<p class="small">Login as student to see joined events.</p>'; return; }
  const u = users.find(x=>x.email === current.email);
  if(!u || !u.joined || u.joined.length === 0){ joinedList.innerHTML = '<p class="small">No joined events yet.</p>'; return; }
  joinedList.innerHTML = u.joined.map(id=>{
    const t = topics.find(x=>x.id===id);
    if(!t) return '';
    return `<div class="topic"><strong>${esc(t.title)}</strong><div class="small">${esc(t.location)}</div><p>${esc(t.desc)}</p></div>`;
  }).join('');
}

/* help button -> mailto admin */
helpBtn.addEventListener('click', ()=>{ window.location.href = 'mailto:' + encodeURIComponent(ADMIN_EMAIL) + '?subject=Language Club Help'; });

/* Save and load helpers called by events */
function saveTopics(){ localStorage.setItem(KEY_TOPICS, JSON.stringify(topics)); }
function saveUsers(){ localStorage.setItem(KEY_USERS, JSON.stringify(users)); }
function saveCurrent(){ localStorage.setItem(KEY_CURR, JSON.stringify(current)); }

/* fix: because saveTopics/saveUsers/name collisions earlier; ensure both functions available */
function saveAll(){
  localStorage.setItem(KEY_TOPICS, JSON.stringify(topics));
  localStorage.setItem(KEY_USERS, JSON.stringify(users));
  if(current) localStorage.setItem(KEY_CURR, JSON.stringify(current));
}

/* ensure saving when adding topics or registering */
function ensureSave(){
  localStorage.setItem(KEY_TOPICS, JSON.stringify(topics));
  localStorage.setItem(KEY_USERS, JSON.stringify(users));
  if(current) localStorage.setItem(KEY_CURR, JSON.stringify(current));
}

/* when current set, persist immediately */
(function autoPersistCurrent(){
  if(current){ localStorage.setItem(KEY_CURR, JSON.stringify(current)); }
})();

/* load from storage on start (already at top) */
renderTopics();
renderUsersList();
renderJoined();
</script>
</body>
</html>

</body>
</html>
