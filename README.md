[
  {<!DOCTYPE html>
<html lang="az">
<head>
  <meta charset="UTF-8">
  <title>Sınaq İmtahanı Nəticələri</title>
  <style>
    body { font-family: Arial, sans-serif; padding: 20px; background: #f4f4f4; }
    .box { background: white; padding: 20px; border-radius: 10px; max-width: 600px; margin: auto; }
    input, button { padding: 10px; margin: 5px; width: 95%; }
    .result { margin-top: 20px; padding: 15px; background: #eef; border-radius: 10px; }
    .correct { color: green; }
    .wrong { color: red; }
  </style>
</head>
<body>
  <div class="box">
    <h2>Sınaq İmtahanı Nəticələri</h2>
    <input type="text" id="exam" placeholder="İmtahan adı (məs: Biologiya)">
    <input type="text" id="id" placeholder="İş nömrəsi (məs: 101)">
    <button onclick="checkResult()">Nəticəni Göstər</button>
    <div id="output" class="result"></div>
  </div>

  <script>
    async function checkResult() {
      let exam = document.getElementById("exam").value.trim();
      let id = document.getElementById("id").value.trim();
      let out = document.getElementById("output");
      out.innerHTML = "Yoxlanılır...";

      const response = await fetch("results.json");
      const data = await response.json();

      let student = data.find(r => r.exam === exam && r.id === id);

      if (!student) {
        out.innerHTML = "<b>Bu məlumat tapılmadı!</b>";
        return;
      }

      let html = `<h3>${student.name}</h3>
                  <p><b>İmtahan:</b> ${student.exam}</p>
                  <p><b>İş №:</b> ${student.id}</p>
                  <hr>`;
      student.answers.forEach((ans, i) => {
        let correct = student.correct[i];
        if (ans === correct) {
          html += `<p>Sual ${i+1}: Cavab – <span class="correct">${ans} ✅</span></p>`;
        } else {
          html += `<p>Sual ${i+1}: Cavab – <span class="wrong">${ans} ❌</span> (Doğru: ${correct})</p>`;
        }
      });
      html += `<hr><b>Ümumi bal: ${student.score}</b>`;
      out.innerHTML = html;
    }
  </script>
</body>
</html>

    "exam": "Biologiya",
    "id": "101",
    "name": "Murad Əliyev",
    "answers": ["B", "C", "D", "A", "B"],
    "correct": ["B", "A", "D", "A", "B"],
    "score": 4
  },
  {
    "exam": "Kimya",
    "id": "102",
    "name": "Aysel Quliyeva",
    "answers": ["A", "B", "C", "D", "C"],
    "correct": ["A", "B", "C", "A", "C"],
    "score": 4
  }
]
