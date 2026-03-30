# My-school-
School management software
<!DOCTYPE html>
<html>
<head>
<title>School Management System</title>

<style>
body {
    font-family: Arial;
    background: linear-gradient(to right, #74ebd5, #ACB6E5);
    text-align: center;
}

.container {
    background: white;
    padding: 20px;
    margin: 20px auto;
    width: 90%;
    border-radius: 10px;
}

input {
    padding: 8px;
    margin: 5px;
    width: 120px;
}

button {
    padding: 8px 12px;
    margin: 5px;
    border: none;
    background: #007bff;
    color: white;
    cursor: pointer;
    border-radius: 5px;
}

button:hover {
    background: #0056b3;
}

table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 20px;
}

th, td {
    border: 1px solid #ddd;
    padding: 10px;
}

th {
    background: #007bff;
    color: white;
}
</style>
</head>

<body>

<div class="container">
<h2>📚 School Management System</h2>

<input type="text" id="name" placeholder="Name">
<input type="text" id="class" placeholder="Class">

<br>

<input type="number" id="math" placeholder="Math">
<input type="number" id="science" placeholder="Science">
<input type="number" id="english" placeholder="English">

<br>

<button onclick="addStudent()">Add Student</button>

<br><br>

<input type="text" id="search" placeholder="Search..." onkeyup="searchStudent()">

<table>
<thead>
<tr>
    <th>Name</th>
    <th>Class</th>
    <th>Marks</th>
    <th>Total</th>
    <th>%</th>
    <th>Grade</th>
    <th>Result</th>
    <th>Action</th>
</tr>
</thead>

<tbody id="list"></tbody>
</table>
</div>

<script>

let students = JSON.parse(localStorage.getItem("students")) || [];

function saveData() {
    localStorage.setItem("students", JSON.stringify(students));
}

function getGrade(p) {
    if (p >= 80) return "A";
    if (p >= 60) return "B";
    if (p >= 40) return "C";
    return "D";
}

function addStudent() {
    let name = document.getElementById("name").value;
    let cls = document.getElementById("class").value;
    let m = Number(document.getElementById("math").value);
    let s = Number(document.getElementById("science").value);
    let e = Number(document.getElementById("english").value);

    if (!name || !cls) return alert("Fill all fields");

    let total = m + s + e;
    let percent = (total / 300) * 100;
    let grade = getGrade(percent);
    let result = percent >= 33 ? "Pass" : "Fail";

    students.push({ name, class: cls, m, s, e, total, percent, grade, result });

    saveData();
    displayStudents();
}

function displayStudents(data = students) {
    let list = document.getElementById("list");
    list.innerHTML = "";

    data.forEach((st, i) => {
        list.innerHTML += `
        <tr>
            <td>${st.name}</td>
            <td>${st.class}</td>
            <td>M:${st.m} S:${st.s} E:${st.e}</td>
            <td>${st.total}</td>
            <td>${st.percent.toFixed(2)}%</td>
            <td>${st.grade}</td>
            <td>${st.result}</td>
            <td>
                <button onclick="showReport(${i})">Report</button>
                <button onclick="deleteStudent(${i})">Delete</button>
            </td>
        </tr>`;
    });
}

function deleteStudent(i) {
    students.splice(i, 1);
    saveData();
    displayStudents();
}

function searchStudent() {
    let val = document.getElementById("search").value.toLowerCase();
    let filtered = students.filter(s => s.name.toLowerCase().includes(val));
    displayStudents(filtered);
}

function showReport(i) {
    let st = students[i];

    let win = window.open("", "", "width=700,height=700");

    win.document.write(`
    <html>
    <head>
    <title>Report Card</title>
    <style>
    body { font-family: Arial; text-align:center; }
    .card { border:2px solid black; padding:20px; margin:20px; }
    table { width:100%; border-collapse: collapse; }
    th,td { border:1px solid black; padding:10px; }
    h2 { background:#007bff; color:white; padding:10px; }
    </style>
    </head>

    <body>

    <div class="card">
    <h2>🏫 School Report Card</h2>

    <p><b>Name:</b> ${st.name}</p>
    <p><b>Class:</b> ${st.class}</p>

    <table>
        <tr><th>Subject</th><th>Marks</th></tr>
        <tr><td>Math</td><td>${st.m}</td></tr>
        <tr><td>Science</td><td>${st.s}</td></tr>
        <tr><td>English</td><td>${st.e}</td></tr>
    </table>

    <h3>Total: ${st.total}</h3>
    <h3>Percentage: ${st.percent.toFixed(2)}%</h3>
    <h3>Grade: ${st.grade}</h3>
    <h3>Result: ${st.result}</h3>

    <br>
    <button onclick="window.print()">Print / Save PDF</button>

    </div>

    </body>
    </html>
    `);
}

displayStudents();

</script>

</body>
</html>