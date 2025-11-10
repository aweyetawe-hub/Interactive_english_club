# Interactive_english_club
learn.speak.grow
<!DOCTYPE html>
<html lang="kk">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Тіл дамыту клубы</title>
    <style>
        body { font-family: Arial, sans-serif; background: #f0f8ff; margin:0; padding:0; }
        header { background:#4682b4; color:white; padding:20px; text-align:center; }
        .container { max-width:600px; margin:30px auto; padding:20px; background:white; border-radius:10px; box-shadow:0 0 10px rgba(0,0,0,0.1);}
        input, button { width:100%; padding:10px; margin:10px 0; border-radius:5px; border:1px solid #ccc; }
        button { background:#4682b4; color:white; border:none; cursor:pointer; }
        button:hover { background:#315f7d; }
        .hidden { display:none; }
        .topic { padding:10px; border-bottom:1px solid #ccc; }
        h3 { margin-top:0; }
    </style>
</head>
<body>

<header>
    <h1>Тіл дамыту клубы</h1>
</header>

<!-- Тіркелу -->
<div class="container" id="registerDiv">
    <h2>Тіркелу</h2>
    <input type="text" id="name" placeholder="Аты-жөніңіз" required>
    <input type="email" id="email" placeholder="Email" required>
    <button onclick="register()">Тіркелу</button>
</div>

<!-- Оқушы панелі -->
<div class="container hidden" id="studentDiv">
    <h2>Сіз тіркелдіңіз!</h2>
    <h3>Апталық тақырыптар</h3>
    <div id="topicsList"></div>
    <button onclick="helpMe()">Көмек</button>
</div>

<!-- Админ панелі -->
<div class="container hidden" id="adminDiv">
    <h2>Админ панелі</h2>
    <h3>Апталық тақырыптарды басқару</h3>
    <div id="topicsListAdmin"></div>
    <input type="text" id="newTopic" placeholder="Жаңа тақырып">
    <button onclick="addTopic()">Қосу</button>
    <button onclick="clearTopics()">Барлығын өшіру</button>
</div>

<script>
    // Алғашқы тақырыптар
    let topics = ["Тілді дамыту мақсатындағы ішкі шараға қатысу"];

    function register() {
        const name = document.getElementById('name').value.trim();
        const email = document.getElementById('email').value.trim();
        if(name === "" || email === "") { alert("Барлық өрістерді толтырыңыз!"); return; }

        localStorage.setItem('userName', name);
        localStorage.setItem('userEmail', email);

        document.getElementById('registerDiv').classList.add('hidden');

        if(email === "aweyetawe@gmail.com") {
            document.getElementById('adminDiv').classList.remove('hidden');
            showTopicsAdmin();
        } else {
            document.getElementById('studentDiv').classList.remove('hidden');
            showTopicsStudent();
        }
    }

    function showTopicsStudent() {
        const list = document.getElementById('topicsList');
        list.innerHTML = "";
        topics.forEach((t, index) => {
            const div = document.createElement('div');
            div.className = "topic";
            div.textContent = `${index + 1}. ${t}`;
            list.appendChild(div);
        });
    }

    function showTopicsAdmin() {
        const list = document.getElementById('topicsListAdmin');
        list.innerHTML = "";
        topics.forEach((t, index) => {
            const div = document.createElement('div');
            div.className = "topic";
            div.textContent = `${index + 1}. ${t}`;
            list.appendChild(div);
        });
    }

    function helpMe() {
        const email = "aweyetawe@gmail.com";
        window.location.href = `mailto:${email}?subject=Көмек&body=Сәлем! Менге көмек қажет.`;
    }

    function addTopic() {
        const newTopic = document.getElementById('newTopic').value.trim();
        if(newTopic === "") { alert("Тақырыпты енгізіңіз!"); return; }
        topics.push(newTopic);
        document.getElementById('newTopic').value = "";
        showTopicsAdmin();
    }

    function clearTopics() {
        if(confirm("Барлық тақырыптарды өшіргіңіз келе ме?")) {
            topics = [];
            showTopicsAdmin();
        }
    }

    window.onload = () => {
        const storedName = localStorage.getItem('userName');
        const storedEmail = localStorage.getItem('userEmail');
        if(storedName && storedEmail) {
            document.getElementById('registerDiv').classList.add('hidden');
            if(storedEmail === "aweyetawe@gmail.com") {
                document.getElementById('adminDiv').classList.remove('hidden');
                showTopicsAdmin();
            } else {
                document.getElementById('studentDiv').classList.remove('hidden');
                showTopicsStudent();
            }
        }
    }
</script>

</body>
</html>
