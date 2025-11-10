# Interactive_english_club
learn.speak.grow
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Language Club</title>
<style>
    body {
        font-family: Arial, sans-serif;
        background-color: #e6f0ff; /* light blue */
        margin: 0;
        padding: 0;
        color: #003366; /* dark blue text */
    }
    header {
        background-color: #003366;
        color: white;
        padding: 10px 20px;
        display: flex;
        align-items: center;
        justify-content: space-between;
    }
    header img {
        height: 50px;
    }
    nav button {
        margin-left: 10px;
        padding: 8px 15px;
        background-color: white;
        color: #003366;
        border: none;
        cursor: pointer;
        border-radius: 5px;
        font-weight: bold;
    }
    nav button:hover {
        background-color: #b3d1ff;
    }
    main {
        padding: 20px;
    }
    .hidden {
        display: none;
    }
    input, textarea, select {
        padding: 8px;
        margin: 5px 0 15px 0;
        width: 100%;
        box-sizing: border-box;
    }
    .btn {
        background-color: #003366;
        color: white;
        border: none;
        padding: 10px 20px;
        cursor: pointer;
        border-radius: 5px;
        font-weight: bold;
    }
    .btn:hover {
        background-color: #002244;
    }
    .topic-card {
        border: 1px solid #003366;
        padding: 15px;
        margin-bottom: 15px;
        border-radius: 8px;
        background-color: white;
    }
</style>
</head>
<body>
<header>
    <img id="logo" src="logo.png" alt="Club Logo">
    <nav>
        <button id="loginBtn">Login/Register</button>
        <button id="helpBtn">Help</button>
    </nav>
</header>

<main>
    <!-- Login/Register -->
    <div id="authDiv">
        <h2>Login / Register</h2>
        <input type="email" id="emailInput" placeholder="Enter your email">
        <input type="password" id="passwordInput" placeholder="Enter password">
        <button class="btn" id="loginRegisterBtn">Login / Register</button>
        <p id="authMsg"></p>
    </div>

    <!-- Admin Panel -->
    <div id="adminDiv" class="hidden">
        <h2>Admin Panel</h2>
        <input type="text" id="newTopic" placeholder="Topic Name">
        <textarea id="newTopicDesc" placeholder="Topic Description"></textarea>
        <input type="text" id="eventLocation" placeholder="Event Location">
        <input type="text" id="logoURL" placeholder="Logo URL (change logo)">
        <button class="btn" id="addTopicBtn">Add Topic</button>
        <h3>Current Topics:</h3>
        <div id="topicsList"></div>
    </div>

    <!-- User Panel -->
    <div id="userDiv" class="hidden">
        <h2>Welcome to Language Club!</h2>
        <div id="userTopicsList"></div>
    </div>
</main>

<script>
const adminEmail = "aweyetawe@gmail.com";

let users = [];
let topics = [];

const authDiv = document.getElementById("authDiv");
const adminDiv = document.getElementById("adminDiv");
const userDiv = document.getElementById("userDiv");

const emailInput = document.getElementById("emailInput");
const passwordInput = document.getElementById("passwordInput");
const authMsg = document.getElementById("authMsg");

const loginRegisterBtn = document.getElementById("loginRegisterBtn");

const newTopic = document.getElementById("newTopic");
const newTopicDesc = document.getElementById("newTopicDesc");
const eventLocation = document.getElementById("eventLocation");
const logoURL = document.getElementById("logoURL");
const addTopicBtn = document.getElementById("addTopicBtn");
const topicsList = document.getElementById("topicsList");
const userTopicsList = document.getElementById("userTopicsList");

const logoImg = document.getElementById("logo");
const helpBtn = document.getElementById("helpBtn");

let currentUser = null;

loginRegisterBtn.addEventListener("click", () => {
    const email = emailInput.value.trim();
    const password = passwordInput.value.trim();
    if(!email || !password) {
        authMsg.textContent = "Please enter email and password.";
        return;
    }

    if(email === adminEmail) {
        currentUser = {email, role: "admin"};
        authDiv.classList.add("hidden");
        adminDiv.classList.remove("hidden");
        renderTopicsAdmin();
        authMsg.textContent = "";
    } else {
        currentUser = {email, role: "user", joined: []};
        users.push(currentUser);
        authDiv.classList.add("hidden");
        userDiv.classList.remove("hidden");
        renderTopicsUser();
        authMsg.textContent = "";
    }
});

addTopicBtn.addEventListener("click", () => {
    const name = newTopic.value.trim();
    const desc = newTopicDesc.value.trim();
    const location = eventLocation.value.trim();
    if(!name || !desc || !location) {
        alert("Please fill all fields.");
        return;
    }
    topics.push({name, desc, location});
    newTopic.value = "";
    newTopicDesc.value = "";
    eventLocation.value = "";
    renderTopicsAdmin();
    renderTopicsUser();
    if(logoURL.value.trim()) {
        logoImg.src = logoURL.value.trim();
    }
});

function renderTopicsAdmin() {
    topicsList.innerHTML = "";
    topics.forEach((t,i) => {
        const div = document.createElement("div");
        div.className = "topic-card";
        div.innerHTML = `<strong>${t.name}</strong><p>${t.desc}</p><p>Location: ${t.location}</p>`;
        topicsList.appendChild(div);
    });
}

function renderTopicsUser() {
    userTopicsList.innerHTML = "";
    topics.forEach((t,i) => {
        const div = document.createElement("div");
        div.className = "topic-card";
        div.innerHTML = `<strong>${t.name}</strong><p>${t.desc}</p><p>Location: ${t.location}</p><button class="btn" onclick="joinTopic(${i})">Join Event</button>`;
        userTopicsList.appendChild(div);
    });
}

function joinTopic(index) {
    if(currentUser.role !== "user") return;
    if(!currentUser.joined.includes(index)) {
        currentUser.joined.push(index);
        alert(`You joined "${topics[index].name}"`);
    } else {
        alert("You already joined this event.");
    }
}

helpBtn.addEventListener("click", () => {
    window.location.href = "mailto:aweyetawe@gmail.com";
});
</script>
</body>
</html>
