# student
student registration
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Student Attendance App</title>

    <style>
        * {
            box-sizing: border-box;
        }

        body {
            font-family: Arial, sans-serif;
            background: #f4f6f8;
            margin: 0;
            padding: 20px;
        }

        .container {
            max-width: 800px;
            margin: auto;
            background: white;
            padding: 25px;
            border-radius: 12px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }

        h1 {
            text-align: center;
            color: #333;
        }

        .add-student {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
        }

        input {
            flex: 1;
            padding: 12px;
            border: 1px solid #ccc;
            border-radius: 6px;
        }

        button {
            padding: 10px 15px;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            color: white;
        }

        .add-btn {
            background: #007bff;
        }

        .present {
            background: #28a745;
        }

        .absent {
            background: #dc3545;
        }

        .delete {
            background: #6c757d;
        }

        table {
            width: 100%;
            border-collapse: collapse;
        }

        th, td {
            padding: 12px;
            text-align: center;
            border-bottom: 1px solid #ddd;
        }

        th {
            background: #007bff;
            color: white;
        }

        .percentage {
            font-weight: bold;
        }

        .empty {
            text-align: center;
            color: #777;
            padding: 20px;
        }
    </style>
</head>

<body>

<div class="container">

    <h1>📚 Student Attendance</h1>

    <div class="add-student">
        <input type="text" id="studentName" placeholder="Enter student name">
        <button class="add-btn" onclick="addStudent()">Add Student</button>
    </div>

    <table>
        <thead>
            <tr>
                <th>#</th>
                <th>Student Name</th>
                <th>Present</th>
                <th>Absent</th>
                <th>Attendance</th>
                <th>Action</th>
            </tr>
        </thead>

        <tbody id="studentList"></tbody>
    </table>

    <div id="emptyMessage" class="empty">
        No students added yet.
    </div>

</div>

<script>
    let students = JSON.parse(localStorage.getItem("students")) || [];

    function saveData() {
        localStorage.setItem("students", JSON.stringify(students));
    }

    function addStudent() {
        const input = document.getElementById("studentName");
        const name = input.value.trim();

        if (name === "") {
            alert("Please enter a student name.");
            return;
        }

        students.push({
            name: name,
            present: 0,
            absent: 0
        });

        input.value = "";

        saveData();
        displayStudents();
    }

    function markPresent(index) {
        students[index].present++;
        saveData();
        displayStudents();
    }

    function markAbsent(index) {
        students[index].absent++;
        saveData();
        displayStudents();
    }

    function deleteStudent(index) {
        if (confirm("Delete this student?")) {
            students.splice(index, 1);
            saveData();
            displayStudents();
        }
    }

    function displayStudents() {
        const list = document.getElementById("studentList");
        const emptyMessage = document.getElementById("emptyMessage");

        list.innerHTML = "";

        if (students.length === 0) {
            emptyMessage.style.display = "block";
            return;
        }

        emptyMessage.style.display = "none";

        students.forEach((student, index) => {

            const total = student.present + student.absent;

            const percentage = total === 0
                ? 0
                : ((student.present / total) * 100).toFixed(1);

            const row = `
                <tr>
                    <td>${index + 1}</td>

                    <td>${student.name}</td>

                    <td>
                        <button class="present"
                                onclick="markPresent(${index})">
                            + Present
                        </button>
                        <br>
                        ${student.present}
                    </td>

                    <td>
                        <button class="absent"
                                onclick="markAbsent(${index})">
                            + Absent
                        </button>
                        <br>
                        ${student.absent}
                    </td>

                    <td class="percentage">
                        ${percentage}%
                    </td>

                    <td>
                        <button class="delete"
                                onclick="deleteStudent(${index})">
                            Delete
                        </button>
                    </td>
                </tr>
            `;

            list.innerHTML += row;
        });
    }

    displayStudents();
</script>

</body>
</html>
