# fajna
<!DOCTYPE html>
<html lang="pl">
<head>
  <meta charset="UTF-8">
  <title>Prosty Excel w przeglądarce</title>
  <style>
    table { border-collapse: collapse; margin-top: 10px; }
    td, th {
      border: 1px solid #999;
      padding: 5px;
      text-align: center;
      min-width: 60px;
    }
    input {
      width: 100%;
      border: none;
      text-align: center;
    }
    input:focus { outline: none; background: #eef; }
  </style>
</head>
<body>
  <h2>Prosty Excel (HTML + JavaScript)</h2>
  <table id="excel"></table>

  <script>
    const cols = ['A','B','C','D','E','F','G','H'];
    const rows = 8;
    const table = document.getElementById('excel');

    // Tworzenie nagłówka
    let header = '<tr><th></th>';
    for (let c of cols) header += `<th>${c}</th>`;
    header += '</tr>';
    table.innerHTML = header;

    // Tworzenie komórek
    for (let r = 1; r <= rows; r++) {
      let row = `<tr><th>${r}</th>`;
      for (let c of cols) {
        const id = c + r;
        row += `<td><input id="${id}" oninput="updateCell('${id}')" /></td>`;
      }
      row += '</tr>';
      table.innerHTML += row;
    }

    // Funkcja przeliczania
    function updateCell(id) {
      const input = document.getElementById(id);
      const value = input.value.trim();

      // tylko jeśli zaczyna się od "="
      if (value.startsWith('=')) {
        try {
          const formula = value.substring(1).toUpperCase();
          const replaced = formula.replace(/[A-H][1-8]/g, ref => {
            const refInput = document.getElementById(ref);
            const val = refInput ? parseFloat(refInput.value) || 0 : 0;
            return val;
          });
          const result = eval(replaced);
          input.value = result;
        } catch (err) {
          input.value = "BŁĄD";
        }
      }
    }
  </script>
</body>
</html>
