<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Kitab'-Ista - Home</title>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Pacifico&family=Roboto:wght@300;400;700&display=swap');

    body {
      font-family: 'Roboto', sans-serif;
      background: url('https://www.transparenttextures.com/patterns/old-moon.png');
      background-color: #fdf6e3;
      background-repeat: repeat;
      margin: 0;
      padding: 0;
    }
    header {
      background-color: #1976d2;
      background-image: url('https://www.transparenttextures.com/patterns/book-cover.png');
      background-size: cover;
      color: white;
      padding: 20px;
      text-align: center;
    }
    h1 {
      font-size: 26px;
      font-family: 'Pacifico', cursive;
    }
    main {
      padding: 20px;
    }
    nav a {
      margin: 10px;
      text-decoration: none;
      color: #0d47a1;
      font-weight: bold;
    }
    nav {
      text-align: center;
      padding: 10px;
      background-color: #bbdefb;
    }
    section {
      margin-bottom: 40px;
    }
    .box {
      background: #ffffff;
      border-radius: 10px;
      box-shadow: 0 4px 8px rgba(0,0,0,0.1);
      padding: 20px;
    }
    input, textarea, button {
      width: 100%;
      padding: 10px;
      margin-top: 10px;
      border-radius: 5px;
      border: 1px solid #ccc;
    }
    button {
      background-color: #1565c0;
      color: white;
      cursor: pointer;
    }
    button:hover {
      background-color: #0d47a1;
    }
    footer {
      text-align: center;
      padding: 20px;
      background-color: #1e88e5;
      color: white;
      font-weight: bold;
      font-family: 'Pacifico', cursive;
    }
    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 20px;
    }
    th, td {
      padding: 10px;
      border: 1px solid #ddd;
      text-align: left;
    }
    .image-dialog {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
    }
    .image-dialog img {
      width: 150px;
      height: 150px;
      object-fit: cover;
      border: 2px solid #ccc;
      border-radius: 8px;
    }
    .admin-box {
      position: fixed;
      top: 10px;
      left: 10px;
      background-color: #ffffffcc;
      border: 2px solid #1565c0;
      border-radius: 50%;
      padding: 15px;
      width: 80px;
      height: 80px;
      z-index: 1000;
      box-shadow: 2px 2px 8px rgba(0,0,0,0.2);
      overflow: hidden;
      text-align: center;
      cursor: pointer;
    }
    .admin-box h3 {
      margin: 0;
      font-size: 1.2em;
      color: #0d47a1;
      line-height: 80px;
    }
  </style>
</head>
<body>
  <div class="admin-box" id="adminPanel" onclick="toggleAdminDetails()">
    <h3>📘 Admin Panel</h3>
    <div style="display: none;" id="adminDetails">
      <p><strong>Admin:</strong> davidsilvia2244@gmail.com</p>
      <label><input type="checkbox" id="verifyCount"> Book count verified</label><br>
      <label for="adminDate">📅 Date:</label>
      <input type="date" id="adminDate" name="adminDate" style="width: 100%; margin-top: 5px;">
      <p><strong>Book Count:</strong> <input type="number" id="bookCountDisplay" min="0" style="width: 60px;"></p>
      <button onclick="exportBookRecords()" style="margin-top: 10px; width: 100%;">📤 Export Book Record</button>
    </div>
  </div>

  <script src="https://cdn.sheetjs.com/xlsx-latest/package/xlsx.full.min.js"></script>
  <script>
    function toggleAdminDetails() {
      const details = document.getElementById('adminDetails');
      details.style.display = (details.style.display === 'none' || details.style.display === '') ? 'block' : 'none';
    }

    const adminAuthEmail = 'davidsilvia2244@gmail.com';
    const currentUserEmail = 'davidsilvia2244@gmail.com';

    if (adminAuthEmail === currentUserEmail) {
      document.getElementById('adminPanel').style.display = 'block';
    }

    const bookCountDisplay = document.getElementById('bookCountDisplay');
    const adminDateInput = document.getElementById('adminDate');

    const table = document.querySelector('#library table tbody');
    if (table && bookCountDisplay) {
      const count = table.getElementsByTagName('tr').length;
      bookCountDisplay.value = count;
    }

    function saveBookRecord() {
      const date = adminDateInput.value;
      const count = bookCountDisplay.value;
      if (date && count) {
        const records = JSON.parse(localStorage.getItem('bookRecords') || '[]');
        records.push({ date, count });
        localStorage.setItem('bookRecords', JSON.stringify(records));
      }
    }

    if (adminDateInput && bookCountDisplay) {
      adminDateInput.addEventListener('change', saveBookRecord);
      bookCountDisplay.addEventListener('change', saveBookRecord);
    }

    function exportBookRecords() {
      const records = JSON.parse(localStorage.getItem('bookRecords') || '[]');
      const worksheet = XLSX.utils.json_to_sheet(records);
      const workbook = XLSX.utils.book_new();
      XLSX.utils.book_append_sheet(workbook, worksheet, 'BookRecords');
      XLSX.writeFile(workbook, 'Kitab-Ista_Book_Counts.xlsx');
    }
  </script>
</body>
</html>
