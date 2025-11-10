# Interactive_english_club
learn.speak.grow
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Interactive Club</title>
  <style>
    body { font-family: Arial, sans-serif; background: #f3f4f6; margin: 0; padding: 0; text-align: center; }
    header { background: #4f46e5; color: white; padding: 20px; }
    main { margin: 20px; }
    input { display: block; margin: 10px auto; padding: 10px; width: 250px; }
    button { padding: 10px 20px; background: #4f46e5; color: white; border: none; cursor: pointer; }
    button:hover { background: #4338ca; }
    .projects { margin-top: 40px; }
  </style>
</head>
<body>
  <header>
    <h1>Welcome to Interactive Club</h1>
  </header>

  <main>
    <section class="register">
      <h2>Register</h2>
      <input type="text" id="name" placeholder="Full Name">
      <input type="email" id="email" placeholder="Email">
      <input type="password" id="password" placeholder="Password">
      <button id="registerBtn">Register</button>
      <p id="registerMessage"></p>
    </section>

    <section class="projects">
      <h2>Available Projects / Lessons</h2>
      <ul id="projectsList"></ul>
    </section>
  </main>

  <script>
    // Тіркелу функциясы
    document.getElementById("registerBtn").addEventListener("click", () => {
      const name = document.getElementById("name").value.trim();
      const email = document.getElementById("email").value.trim();
      const password = document.getElementById("password").value.trim();

      if (!name || !email || !password) {
        document.getElementById("registerMessage").innerText = "Барлық өрістерді толтырыңыз!";
        return;
      }

      let users = JSON.parse(localStorage.getItem("users") || "[]");
      if (users.some(user => user.email === email)) {
        document.getElementById("registerMessage").innerText = "Бұл email бұрын тіркелген!";
        return;
      }

      users.push({ name, email, password });
      localStorage.setItem("users", JSON.stringify(users));
      document.getElementById("registerMessage").innerText = "Сәтті тіркелдіңіз!";
      document.getElementById("name").value = "";
      document.getElementById("email").value = "";
      document.getElementById("password").value = "";
    });

    // Сабақтар / жобалар тізімі
    const projects = ["Math Club", "Science Workshop", "Art Lesson", "English Debate"];
    const projectsList = document.getElementById("projectsList");
    projects.forEach(proj => {
      const li = document.createElement("li");
      li.innerText = proj;
      projectsList.appendChild(li);
    });
  </script>
</body>
</html>
