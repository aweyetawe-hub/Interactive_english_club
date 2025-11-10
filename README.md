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
            background-color: #e6f0ff;
            color: #003366;
            margin: 0;
            padding: 0;
        }
        header {
            background-color: #003366;
            color: white;
            padding: 10px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        header img {
            height: 50px;
            cursor: pointer;
        }
        main {
            padding: 20px;
        }
        .hidden { display: none; }
        .button {
            background-color: #003366;
            color: white;
            padding: 10px 15px;
            border: none;
            cursor: pointer;
            margin: 5px;
            border-radius: 5px;
        }
        .input-field {
            padding: 8px;
            margin: 5px 0;
            width: 100%;
            box-sizing: border-box;
        }
        .topic-card {
            border: 1px solid #003366;
            border-radius: 5px;
            padding: 15px;
            margin: 10px 0;
            background-color: white;
        }
        .flex { display: flex; gap: 10px; }
    </style>
</head>
<body>
    <header>
        <img id="logo" src="https://via.placeholder.com/100x50.png?text=Logo" alt="Logo">
        <div>
            <button class="button" id="loginBtn">Login</button>
        </div>
    </header>

    <main>
        <!-- Login Section -->
        <div id="loginSection">
            <h2>Login</h2>
            <input type="email" id="email" class="input-field" placeholder="Email">
            <button class="button" id="enterBtn">Enter</button>
        </div>

        <!-- Admin Section -->
        <div id="adminSection" class="hidden">
            <h2>Admin Panel</h2>
            <label>Topic Name:</label>
            <input type="text" id="topicName" class="input-field">
            <label>Topic Description:</label>
            <textarea id="topicDesc" class="input-field"></textarea>
            <label>Emblem URL:</label>
            <input type="text" id="emblemUrl" class="input-field">
            <button class="button" id="addTopicBtn">Add Topic</button>
            <h3>Current Topics</h3>
            <div id="topicsList"></div>
        </div>

        <!-- Student Section -->
        <div id="studentSection" class="hidden">
            <h2>Available Topics</h2>
            <div id="studentTopics"></div>
            <button class="button" id="helpBtn">Need Help?</button>
        </div>
    </main>

    <script>
        const adminEmail = 'aweyetawe@gmail.com';

        const loginSection = document.getElementById('loginSection');
        const adminSection = document.getElementById('adminSection');
        const studentSection = document.getElementById('studentSection');

        const emailInput = document.getElementById('email');
        const enterBtn = document.getElementById('enterBtn');

        const topicNameInput = document.getElementById('topicName');
        const topicDescInput = document.getElementById('topicDesc');
        const emblemUrlInput = document.getElementById('emblemUrl');
        const addTopicBtn = document.getElementById('addTopicBtn');
        const topicsList = document.getElementById('topicsList');
        const studentTopics = document.getElementById('studentTopics');

        const helpBtn = document.getElementById('helpBtn');
        const logo = document.getElementById('logo');

        let topics = [];

        enterBtn.onclick = () => {
            const email = emailInput.value.trim();
            if(email === adminEmail){
                loginSection.classList.add('hidden');
                adminSection.classList.remove('hidden');
            } else if(email){
                loginSection.classList.add('hidden');
                studentSection.classList.remove('hidden');
                renderStudentTopics();
            } else {
                alert('Please enter an email');
            }
        };

        addTopicBtn.onclick = () => {
            const name = topicNameInput.value.trim();
            const desc = topicDescInput.value.trim();
            const emblem = emblemUrlInput.value.trim() || 'https://via.placeholder.com/100x50.png?text=Logo';
            if(name && desc){
                topics.push({name, desc, emblem});
                renderAdminTopics();
                renderStudentTopics();
                topicNameInput.value = '';
                topicDescInput.value = '';
                emblemUrlInput.value = '';
            } else {
                alert('Please fill in all fields');
            }
        };

        function renderAdminTopics(){
            topicsList.innerHTML = '';
            topics.forEach((topic, index) => {
                const div = document.createElement('div');
                div.className = 'topic-card';
                div.innerHTML = `<img src='${topic.emblem}' height='50'><h4>${topic.name}</h4><p>${topic.desc}</p>`;
                topicsList.appendChild(div);
            });
        }

        function renderStudentTopics(){
            studentTopics.innerHTML = '';
            topics.forEach((topic, index) => {
                const div = document.createElement('div');
                div.className = 'topic-card';
                div.innerHTML = `<img src='${topic.emblem}' height='50'><h4>${topic.name}</h4><p>${topic.desc}</p><button class='button' onclick='joinEvent(${index})'>Join Event</button>`;
                studentTopics.appendChild(div);
            });
        }

        function joinEvent(index){
            alert(`You have joined: ${topics[index].name}`);
        }

        helpBtn.onclick = () => {
            window.location.href = 'mailto:aweyetawe@gmail.com';
        };

        logo.onclick = () => {
            alert('This is the Language Club');
        };
    </script>
</body>
</html>

