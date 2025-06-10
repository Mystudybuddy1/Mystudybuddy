<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>MyStudyBuddy</title>
  <link href="https://fonts.googleapis.com/css2?family=Comic+Neue:wght@700&family=Roboto&display=swap" rel="stylesheet">
  <style>
    body {
      font-family: 'Roboto', sans-serif;
      background: linear-gradient(to right, #f9f9ff, #e0f7fa);
      color: #333;
      margin: 0;
      padding: 20px;
      max-width: 800px;
      margin: auto;
    }
    h1, h2 {
      font-family: 'Comic Neue', cursive;
      color: #2c3e50;
    }
    input, button, textarea {
      width: 100%;
      padding: 10px;
      margin: 5px 0 15px;
      border: 1px solid #ccc;
      border-radius: 8px;
      box-sizing: border-box;
      font-family: 'Roboto', sans-serif;
    }
    button {
      background-color: #00796b;
      color: white;
      font-weight: bold;
      cursor: pointer;
      transition: background-color 0.3s;
    }
    button:hover {
      background-color: #004d40;
    }
    .section {
      background-color: #ffffff;
      padding: 15px;
      border-radius: 12px;
      margin-bottom: 20px;
      box-shadow: 0 4px 6px rgba(0,0,0,0.1);
    }
    .completed span {
      text-decoration: line-through;
      color: gray;
    }
    .homework-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 10px;
    }
    #timer-display {
      font-size: 1.5em;
      text-align: center;
      color: #00796b;
    }
    .note-area {
      resize: vertical;
      min-height: 100px;
    }
    .dark-mode {
      background: #212121;
      color: #f4f4f4;
    }
    .dark-mode .section {
      background: #424242;
    }
    .toggle-btn {
      float: right;
      margin-top: -40px;
      margin-right: 10px;
    }
  </style>
</head>
<body>
  <h1>🎓 MyStudyBuddy</h1>
  <button class="toggle-btn" onclick="toggleDarkMode()">🌙 Toggle Dark Mode</button>

  <div class="section">
    <h2>📚 Homework Tracker</h2>
    <input id="subject" placeholder="Subject" />
    <input id="task" placeholder="Homework Task" />
    <input id="due" placeholder="Due Date (e.g. June 15)" />
    <button onclick="addHomework()">➕ Add Homework</button>
    <div id="homework-list"></div>
    <button onclick="downloadHomework()">⬇️ Download Homework</button>
  </div>

  <div class="section">
    <h2>📖 Word of the Day</h2>
    <button onclick="showWord()">Show Word</button>
    <div id="word-section"></div>
  </div>

  <div class="section">
    <h2>⏰ Study Timer</h2>
    <input id="minutes" placeholder="Enter minutes" type="number" min="1" />
    <button onclick="startTimer()">Start Timer</button>
    <div id="timer-display"></div>
  </div>

  <div class="section">
    <h2>🧮 Maths Table Quiz</h2>
    <input id="table-number" placeholder="Enter a number (e.g. 2)" type="number" />
    <button onclick="startTableQuiz()">Start Quiz</button>
    <div id="quiz-section"></div>
  </div>

  <div class="section">
    <h2>📝 Notes</h2>
    <textarea id="notes" class="note-area" placeholder="Write your notes here..."></textarea>
    <button onclick="saveNotes()">💾 Save Notes</button>
    <button onclick="downloadNotes()">⬇️ Download Notes</button>
  </div>

  <script>
    const words = [...]; // (same word list)

    function addHomework() {
      const subject = document.getElementById("subject").value.trim();
      const task = document.getElementById("task").value.trim();
      const due = document.getElementById("due").value.trim();
      if (subject && task && due) {
        const div = document.createElement("div");
        div.classList.add("homework-item");
        div.innerHTML = `<span>${subject} - ${task} (Due: ${due})</span><button onclick="toggleDone(this)">✔</button>`;
        document.getElementById("homework-list").appendChild(div);
        document.getElementById("subject").value = "";
        document.getElementById("task").value = "";
        document.getElementById("due").value = "";
      } else {
        alert("Please fill all homework fields.");
      }
    }

    function toggleDone(button) {
      button.parentElement.classList.toggle("completed");
    }

    function downloadHomework() {
      const list = document.getElementById("homework-list").innerText;
      const blob = new Blob([list], { type: 'text/plain' });
      const a = document.createElement('a');
      a.href = URL.createObjectURL(blob);
      a.download = 'homework.txt';
      a.click();
    }

    function showWord() {
      const word = words[Math.floor(Math.random() * words.length)];
      document.getElementById("word-section").innerHTML = `<p><strong>Word:</strong> ${word.word}</p><p><strong>Meaning:</strong> ${word.meaning}</p><p><strong>Example:</strong> <em>${word.example}</em></p>`;
    }

    function startTimer() {
      let minutes = parseInt(document.getElementById("minutes").value);
      if (isNaN(minutes) || minutes <= 0) {
        alert("Enter valid study time in minutes.");
        return;
      }
      let totalSeconds = minutes * 60;
      const display = document.getElementById("timer-display");
      const interval = setInterval(() => {
        const mins = Math.floor(totalSeconds / 60);
        const secs = totalSeconds % 60;
        display.textContent = `${mins.toString().padStart(2, '0')}:${secs.toString().padStart(2, '0')}`;
        totalSeconds--;
        if (totalSeconds < 0) {
          clearInterval(interval);
          alert("Time's up! Take a short break.");
        }
      }, 1000);
    }

    function startTableQuiz() {
      const num = parseInt(document.getElementById("table-number").value);
      if (isNaN(num)) return alert("Enter a valid number");
      let html = "<ol>";
      for (let i = 1; i <= 10; i++) {
        html += `<li>${num} x ${i} = ${num * i}</li>`;
      }
      html += "</ol>";
      document.getElementById("quiz-section").innerHTML = html;
    }

    function saveNotes() {
      localStorage.setItem("myNotes", document.getElementById("notes").value);
      alert("Notes saved locally.");
    }

    function downloadNotes() {
      const text = document.getElementById("notes").value;
      const blob = new Blob([text], { type: 'text/plain' });
      const a = document.createElement('a');
      a.href = URL.createObjectURL(blob);
      a.download = 'notes.txt';
      a.click();
    }

    function toggleDarkMode() {
      document.body.classList.toggle("dark-mode");
    }

    // Load notes if saved before
    window.onload = function() {
      const saved = localStorage.getItem("myNotes");
      if (saved) document.getElementById("notes").value = saved;
    }
  </script>
</body>
</html>
