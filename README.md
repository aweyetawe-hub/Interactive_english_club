# Interactive_english_club
learn.speak.grow
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Student Panel</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <header>
        <img id="logo" src="logo.png" alt="Club Logo" class="logo">
        <h1>Welcome to Our English Club</h1>
    </header>

    <div class="register">
        <h2>Student Registration</h2>
        <input type="text" id="name" placeholder="Your Name">
        <input type="email" id="email" placeholder="Your Email">
        <button onclick="registerStudent()">Register</button>
    </div>

    <div class="topics" style="display:none;">
        <h2>Weekly Topics</h2>
        <ul id="topicList"></ul>
    </div>

    <div class="help" style="display:none;">
        <button onclick="openChat()">Help</button>
        <div id="chatBox" style="display:none;">
            <div id="messages" class="messages"></div>
            <input type="text" id="chatInput" placeholder="Type your message">
            <button onclick="sendMessage()">Send</button>
        </div>
    </div>

    <script>
        let topics = JSON.parse(localStorage.getItem("topics")) || [];
        let messages = JSON.parse(localStorage.getItem("messages")) || [];

        function registerStudent(){
            let name = document.getElementById("name").value;
            let email = document.getElementById("email").value;
            if(!name || !email){
                alert("Please enter name and email!");
                return;
            }
            localStorage.setItem("studentName", name);
            localStorage.setItem("studentEmail", email);
            alert("Registered successfully!");
            showTopics();
        }

        function showTopics(){
            document.querySelector(".register").style.display = "none";
            document.querySelector(".topics").style.display = "block";
            document.querySelector(".help").style.display = "block";

            let list = document.getElementById("topicList");
            list.innerHTML = "";
            topics.forEach((t, i)=>{
                let li = document.createElement("li");
                li.innerHTML = `<b>${t.title}</b>: ${t.description} 
                <button onclick="joinEvent(${i})">I want to join</button>`;
                list.appendChild(li);
            });
        }

        function joinEvent(index){
            alert(`You have joined: ${topics[index].title}`);
        }

        function openChat(){
            document.getElementById("chatBox").style.display = "block";
            renderMessages();
        }

        function sendMessage(){
            let input = document.getElementById("chatInput");
            if(input.value.trim() === "") return;
            messages.push({sender: localStorage.getItem("studentName") || "Student", text: input.value});
            localStorage.setItem("messages", JSON.stringify(messages));
            input.value = "";
            renderMessages();
        }

        function renderMessages(){
            let box = document.getElementById("messages");
            box.innerHTML = "";
            messages.forEach(msg=>{
                let div = document.createElement("div");
                div.textContent = msg.sender + ": " + msg.text;
                box.appendChild(div);
            });
        }

        if(localStorage.getItem("studentName")){
            showTopics();
        }
    </script>
</body>
</html>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Admin Panel</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <header>
        <img id="logo" src="logo.png" alt="Club Logo" class="logo">
        <h1>Admin Panel</h1>
    </header>

    <div class="login">
        <input type="email" id="adminEmail" placeholder="Admin Email">
        <button onclick="loginAdmin()">Login</button>
    </div>

    <div class="admin" style="display:none;">
        <h2>Add Weekly Topic</h2>
        <input type="text" id="topicTitle" placeholder="Topic Title">
        <textarea id="topicDesc" placeholder="Topic Description"></textarea>
        <button onclick="addTopic()">Add Topic</button>

        <h2>Change Logo</h2>
        <input type="file" id="logoFile">
        <button onclick="changeLogo()">Update Logo</button>

        <h2>Help Chat</h2>
        <button onclick="openChat()">Open Student Messages</button>
        <div id="chatBox" style="display:none;">
            <div id="messages" class="messages"></div>
            <input type="text" id="chatInput" placeholder="Type your reply">
            <button onclick="sendMessage()">Send</button>
        </div>
    </div>

    <script>
        const adminEmail = "aweyetawe@gmail.com";
        let topics = JSON.parse(localStorage.getItem("topics")) || [];
        let messages = JSON.parse(localStorage.getItem("messages")) || [];

        function loginAdmin(){
            let email = document.getElementById("adminEmail").value;
            if(email !== adminEmail){
                alert("Access denied!");
                return;
            }
            document.querySelector(".login").style.display = "none";
            document.querySelector(".admin").style.display = "block";
        }

        function addTopic(){
            let title = document.getElementById("topicTitle").value;
            let desc = document.getElementById("topicDesc").value;
            if(!title || !desc){
                alert("Enter title and description");
                return;
            }
            topics.push({title:title, description:desc});
            localStorage.setItem("topics", JSON.stringify(topics));
            alert("Topic added!");
        }

        function changeLogo(){
            const file = document.getElementById("logoFile").files[0];
            if(file){
                const reader = new FileReader();
                reader.onload = function(e){
                    document.getElementById("logo").src = e.target.result;
                    localStorage.setItem("logo", e.target.result);
                }
                reader.readAsDataURL(file);
            }
        }

        function openChat(){
            document.getElementById("chatBox").style.display = "block";
            renderMessages();
        }

        function sendMessage(){
            let input = document.getElementById("chatInput");
            if(input.value.trim() === "") return;
            messages.push({sender:"Admin", text: input.value});
            localStorage.setItem("messages", JSON.stringify(messages));
            input.value = "";
            renderMessages();
        }

        function renderMessages(){
            let box = document.getElementById("messages");
            box.innerHTML = "";
            messages.forEach(msg=>{
                let div = document.createElement("div");
                div.textContent = msg.sender + ": " + msg.text;
                box.appendChild(div);
            });
        }

        // restore logo if exists
        const savedLogo = localStorage.getItem("logo");
        if(savedLogo){
            document.getElementById("logo").src = savedLogo;
        }
    </script>
</body>
</html>
body{
    font-family: Arial, sans-serif;
    background-color: #f0f8ff;
    color: #003366;
    text-align: center;
}

header{
    background-color: #003366;
    color: white;
    padding: 20px;
}

.logo{
    width: 100px;
}

input, textarea, button{
    padding: 10px;
    margin: 5px;
    border-radius: 5px;
}

button{
    background-color: #003366;
    color: white;
    border: none;
    cursor: pointer;
}

button:hover{
    background-color: #0055aa;
}

.messages{
    border:1px solid #003366;
    height:200px;
    overflow-y:auto;
    margin:10px;
    padding:10px;
    background-color:white;
}
0
