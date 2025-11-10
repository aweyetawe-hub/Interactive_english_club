# Interactive_english_club
learn.speak.grow
// Simple client-side app using localStorage
const STORAGE_USERS = 'iec_users_v1';
const STORAGE_PROJECTS = 'iec_projects_v1';

function $(sel){return document.querySelector(sel);}
function $all(sel){return document.querySelectorAll(sel);}

function loadUsers(){return JSON.parse(localStorage.getItem(STORAGE_USERS) || '[]');}
function saveUsers(u){localStorage.setItem(STORAGE_USERS, JSON.stringify(u));}

function loadProjects(){
  let p = JSON.parse(localStorage.getItem(STORAGE_PROJECTS) || 'null');
  if(!p){
    // default demo projects
    p = [
      {id: 'p1', title: 'Speaking Practice', date: '2025-11-15', desc: 'Everyday communication. 16:00 - 17:00', seats: 15, joined: []},
      {id: 'p2', title: 'Vocabulary Games', date: '2025-11-22', desc: 'Fun games to expand vocab', seats: 12, joined: []},
      {id: 'p3', title: 'Drama Club', date: '2025-11-29', desc: 'Role-plays and short sketches', seats: 10, joined: []}
    ];
    localStorage.setItem(STORAGE_PROJECTS, JSON.stringify(p));
  }
  return JSON.parse(localStorage.getItem(STORAGE_PROJECTS));
}
function saveProjects(p){localStorage.setItem(STORAGE_PROJECTS, JSON.stringify(p));}

function registerUser(e){
  e.preventDefault();
  const fullName = $('#fullName').value.trim();
  const grade = $('#grade').value;
  const email = $('#email').value.trim();
  const prefLang = $('#prefLang').value;
  if(!fullName || !grade || !email) { alert('Please complete required fields'); return; }
  const users = loadUsers();
  if(users.find(u=>u.email===email)){ alert('This email already registered.'); return; }
  const user = {id: 'u'+Date.now(), fullName, grade, email, prefLang, joined: []};
  users.push(user);
  saveUsers(users);
  // store current session
  localStorage.setItem('iec_current_user', user.id);
  alert('Registration successful! Welcome, ' + fullName);
  window.location.href = 'dashboard.html';
}

function initRegister(){
  const form = $('#regForm');
  if(form) form.addEventListener('submit', registerUser);
}

function renderProjectsList(){
  const list = $('#projectsList');
  if(!list) return;
  const projects = loadProjects();
  list.innerHTML = '';
  projects.forEach(p=>{
    const div = document.createElement('div'); div.className='event card';
    div.innerHTML = `<div>
      <strong>${p.title}</strong><div class="small">${p.date} â¢ ${p.desc}</div>
    </div>
    <div>
      <div class="small">Seats: ${p.seats - p.joined.length}</div>
      <button class="button joinBtn" data-id="${p.id}">Join</button>
    </div>`;
    list.appendChild(div);
  });
  $all('.joinBtn').forEach(b=>b.addEventListener('click', joinProject));
}

function joinProject(e){
  const id = e.currentTarget.dataset.id;
  const cur = localStorage.getItem('iec_current_user');
  if(!cur){ if(confirm('You must register first. Go to Register page?')) window.location.href='register.html'; return; }
  const users = loadUsers();
  const user = users.find(u=>u.id===cur);
  if(!user){ alert('User not found'); return; }
  const projects = loadProjects();
  const project = projects.find(p=>p.id===id);
  if(project.joined.includes(user.id)){ alert('You already joined this event'); return; }
  if(project.joined.length >= project.seats){ alert('No seats available'); return; }
  project.joined.push(user.id);
  user.joined.push(project.id);
  saveProjects(projects);
  saveUsers(users);
  alert('You have successfully joined "'+project.title+'"');
  renderProjectsList();
  if(window.location.pathname.endsWith('dashboard.html')) renderDashboard();
}

function renderDashboard(){
  const cur = localStorage.getItem('iec_current_user');
  if(!cur){ $('#profileArea').innerHTML = '<p>Please register first. <a href="register.html">Register</a></p>'; return; }
  const users = loadUsers();
  const user = users.find(u=>u.id===cur);
  if(!user) return;
  $('#profileName').textContent = user.fullName;
  const pArea = $('#myProjects'); pArea.innerHTML = '';
  const projects = loadProjects();
  user.joined.forEach(pid=>{
    const proj = projects.find(p=>p.id===pid);
    if(!proj) return;
    const div = document.createElement('div'); div.className='event card';
    div.innerHTML = `<div><strong>${proj.title}</strong><div class="small">${proj.date}</div></div>
      <div><button class="button leaveBtn" data-id="${proj.id}">Leave</button></div>`;
    pArea.appendChild(div);
  });
  $all('.leaveBtn').forEach(b=>{
    b.addEventListener('click', function(e){
      const pid = e.currentTarget.dataset.id;
      if(!confirm('Leave this project?')) return;
      let users = loadUsers(); let projects = loadProjects();
      const user = users.find(u=>u.id===localStorage.getItem('iec_current_user'));
      const proj = projects.find(p=>p.id===pid);
      proj.joined = proj.joined.filter(x=>x!==user.id);
      user.joined = user.joined.filter(x=>x!==pid);
      saveProjects(projects); saveUsers(users);
      renderDashboard();
      alert('You left the project.');
    });
  });
}

document.addEventListener('DOMContentLoaded', function(){
  initRegister();
  renderProjectsList();
  renderDashboard();
});
