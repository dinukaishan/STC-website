<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Attendance System</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; background-color: #f4f4f9; }
        h2 { text-align: center; color: #333; }
        form { background: white; padding: 20px; border-radius: 8px; box-shadow: 0 0 10px rgba(0, 0, 0, 0.1); max-width: 500px; margin: auto; }
        label { display: block; margin-top: 10px; font-weight: bold; }
        input { width: 100%; padding: 8px; margin-top: 5px; border: 1px solid #ddd; border-radius: 4px; }
        button { margin-top: 10px; padding: 10px; background: #28a745; color: white; border: none; border-radius: 4px; cursor: pointer; width: 100%; }
        button:hover { background: #218838; }
        table { width: 100%; border-collapse: collapse; margin-top: 20px; background: white; border-radius: 8px; overflow: hidden; }
        th, td { border: 1px solid #ddd; padding: 10px; text-align: left; }
        th { background-color: #007bff; color: white; }
        tr:nth-child(even) { background-color: #f2f2f2; }
        .container { max-width: 900px; margin: auto; padding: 20px; }
        footer { text-align: right; font-size: small; margin-top: 20px; color: #555; }
        @media (max-width: 600px) {
            form, .container { width: 100%; padding: 10px; }
            table { font-size: 14px; }
        }
    </style>
</head>
<body>
    <div class="container">
        <h2>Attendance Form</h2>
        <form id="attendanceForm">
            <label>EPF No: <input type="text" id="epfNo" required></label>
            <label>Employee Name: <input type="text" id="employeeName" required></label>
            <label>Time In: <input type="time" id="timeIn" required></label>
            <label>Time Out: <input type="time" id="timeOut" required></label>
            <label>Attendance Date: <input type="date" id="attendanceDate" required></label>
            <label>Visit Tip Message Bar: <input type="text" id="visitTip"></label>
            <label>Attendance Status: <input type="text" id="attendanceStatus" required></label>
            <button type="submit">Add Attendance</button>
        </form>

        <h2>Attendance Records</h2>
        <table id="attendanceTable">
            <thead>
                <tr>
                    <th>EPF No</th>
                    <th>Employee Name</th>
                    <th>Time In</th>
                    <th>Time Out</th>
                    <th>Attendance Date</th>
                    <th>Visit Tip Message</th>
                    <th>Attendance Status</th>
                    <th>Action</th>
                </tr>
            </thead>
            <tbody></tbody>
        </table>
        
        <button onclick="exportCSV()">Export to CSV</button>
        <button onclick="exportPDF()">Export to PDF</button>
    </div>
    
    <footer>Prepared by Mandira Amandi</footer>
    
    <script>
        document.getElementById("attendanceForm").addEventListener("submit", function(event) {
            event.preventDefault();
            let epfNo = document.getElementById("epfNo").value;
            let employeeName = document.getElementById("employeeName").value;
            let timeIn = document.getElementById("timeIn").value;
            let timeOut = document.getElementById("timeOut").value;
            let attendanceDate = document.getElementById("attendanceDate").value;
            let visitTip = document.getElementById("visitTip").value;
            let attendanceStatus = document.getElementById("attendanceStatus").value;

            let record = { epfNo, employeeName, timeIn, timeOut, attendanceDate, visitTip, attendanceStatus };
            let records = JSON.parse(localStorage.getItem("attendanceRecords")) || [];
            records.push(record);
            localStorage.setItem("attendanceRecords", JSON.stringify(records));
            displayRecords();
            document.getElementById("attendanceForm").reset();
        });

        function displayRecords() {
            let records = JSON.parse(localStorage.getItem("attendanceRecords")) || [];
            let tableBody = document.querySelector("#attendanceTable tbody");
            tableBody.innerHTML = "";
            records.forEach((record, index) => {
                let row = tableBody.insertRow();
                row.innerHTML = `
                    <td>${record.epfNo}</td>
                    <td>${record.employeeName}</td>
                    <td>${record.timeIn}</td>
                    <td>${record.timeOut}</td>
                    <td>${record.attendanceDate}</td>
                    <td>${record.visitTip}</td>
                    <td>${record.attendanceStatus}</td>
                    <td><button onclick="deleteRecord(${index})">Delete</button></td>
                `;
            });
        }

        function deleteRecord(index) {
            let password = prompt("Enter password to delete record:");
            if (password === "1106") {
                let records = JSON.parse(localStorage.getItem("attendanceRecords"));
                records.splice(index, 1);
                localStorage.setItem("attendanceRecords", JSON.stringify(records));
                displayRecords();
            } else {
                alert("Incorrect password!");
            }
        }

        function exportCSV() {
            let records = JSON.parse(localStorage.getItem("attendanceRecords")) || [];
            let csvContent = "data:text/csv;charset=utf-8,EPF No,Employee Name,Time In,Time Out,Attendance Date,Visit Tip Message,Attendance Status\n";
            records.forEach(record => {
                csvContent += `${record.epfNo},${record.employeeName},${record.timeIn},${record.timeOut},${record.attendanceDate},${record.visitTip},${record.attendanceStatus}\n`;
            });
            let encodedUri = encodeURI(csvContent);
            let link = document.createElement("a");
            link.setAttribute("href", encodedUri);
            link.setAttribute("download", "attendance_records.csv");
            document.body.appendChild(link);
            link.click();
        }

        function exportPDF() {
            alert("PDF export functionality to be implemented.");
        }

        displayRecords();
    </script>
</body>
</html>
