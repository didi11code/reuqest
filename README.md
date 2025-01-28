<!DOCTYPE html>
<html lang="he">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>בחינת בקשת אשראי</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            direction: rtl;
            margin: 0;
            padding: 20px;
            background-color: #f4f4f4;
        }
        .container {
            max-width: 700px;
            margin: 0 auto;
            background-color: #fff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
        }
        h1 {
            text-align: center;
            color: #333;
        }
        label {
            display: block;
            margin-bottom: 8px;
            font-weight: bold;
        }
        input[type="text"], input[type="number"], textarea, select {
            width: 100%;
            padding: 10px;
            margin-bottom: 20px;
            border: 1px solid #ccc;
            border-radius: 4px;
        }
        button {
            width: 100%;
            padding: 10px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }
        button:hover {
            background-color: #45a049;
        }
        .result {
            margin-top: 20px;
            text-align: center;
            padding: 10px;
            background-color: #f9f9f9;
            border: 1px solid #ccc;
            border-radius: 4px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>בקשת אשראי</h1>
        <form id="credit-form">
            <label for="full-name">שם מלא:</label>
            <input type="text" id="full-name" required>

            <label for="customer-type">סוג לקוח:</label>
            <select id="customer-type" required>
                <option value="new">לקוח חדש</option>
                <option value="existing">לקוח ותיק</option>
            </select>

            <label for="income">הכנסה שנתית (בש"ח):</label>
            <input type="number" id="income" required>

            <label for="ebitda">EBITDA (בש"ח):</label>
            <input type="number" id="ebitda" required>

            <label for="equity">הון עצמי (בש"ח):</label>
            <input type="number" id="equity" required>

            <label for="credit-score">דירוג אשראי:</label>
            <input type="number" id="credit-score" required>

            <label for="existing-debts">חובות קיימים (בש"ח):</label>
            <input type="number" id="existing-debts" required>

            <label for="loan-amount">סכום הלוואה מבוקש (בש"ח):</label>
            <input type="number" id="loan-amount" required>

            <label for="loan-purpose">מטרת האשראי:</label>
            <select id="loan-purpose" required>
                <option value="working-capital">הון חוזר</option>
                <option value="investment-car">השקעה או רכישת רכב</option>
            </select>

            <label for="loan-term">פריסת הלוואה (בשנים):</label>
            <input type="number" id="loan-term" min="1" max="5" value="5" required>

            <label for="loan-description">הסבר על מטרת האשראי:</label>
            <textarea id="loan-description" rows="4" required></textarea>

            <button type="button" onclick="checkCreditRequest()">בדוק בקשה</button>
            <button type="button" onclick="downloadPDF()">הורד כ-PDF</button>
        </form>

        <div id="result" class="result" style="display: none;"></div>
    </div>

    <!-- הוספת ספריית jsPDF -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

    <script>
        const { jsPDF } = window.jspdf;

        // פונקציה לבדוק את בקשת האשראי
        function checkCreditRequest() {
            var fullName = document.getElementById('full-name').value;
            var customerType = document.getElementById('customer-type').value;
            var income = parseFloat(document.getElementById('income').value);
            var ebitda = parseFloat(document.getElementById('ebitda').value);
            var equity = parseFloat(document.getElementById('equity').value);
            var creditScore = parseInt(document.getElementById('credit-score').value);
            var existingDebts = parseFloat(document.getElementById('existing-debts').value);
            var loanAmount = parseFloat(document.getElementById('loan-amount').value);
            var loanPurpose = document.getElementById('loan-purpose').value;
            var loanTerm = parseInt(document.getElementById('loan-term').value);
            var loanDescription = document.getElementById('loan-description').value;
            var resultDiv = document.getElementById('result');

            // התנאים הכלליים של הבקשה
            var resultText = "";
            var isApproved = false;

            if (income >= 5000 && creditScore >= 600 && loanAmount <= income * 5) {
                resultText = "<p style='color: green;'>הבקשה מאושרת!</p>";
                isApproved = true;
            } else {
                resultText = "<p style='color: red;'>הבקשה נדחתה. יש לבדוק את הדרישות.</p>";
            }

            resultDiv.innerHTML = resultText;
            resultDiv.style.display = "block";
        }

        // פונקציה להורדת קובץ PDF
        function downloadPDF() {
            const doc = new jsPDF({
                orientation: 'p',
                unit: 'mm',
                format: 'a4'
            });

            // הגדרת גופן עברי
            doc.addFileToVFS("David.ttf", "AAEAAAASAQAABAAwQ1F1cHlQqZR5BXln4FLu4lndFTFCO9A3oHRtrh-0mPaCkNKmq0wI2MQYd3g6t8_JtI5ZZxTaT8EgkC5UL2yY7OBOyfbSSt7hS-cKM8oTOz5tJY9tklJ7hzT5qS9FfgIYRL_rnlI25Kh8seNoQHkpft1rcAtz0OxlMwvoh5vcTws6_GV9A==");
            doc.addFont("David.ttf", "David", "normal");

            doc.setFont("David");
            doc.setFontSize(12);

            // הוספת תוכן ב-PDF
            doc.text(`שם מלא: ${document.getElementById('full-name').value}`, 10, 20);
            doc.text(`סוג לקוח: ${document.getElementById('customer-type').value}`, 10, 30);
            doc.text(`הכנסה שנתית: ${document.getElementById('income').value} ש"ח`, 10, 40);
            doc.text(`EBITDA: ${document.getElementById('ebitda').value} ש"ח`, 10, 50);
            doc.text(`הון עצמי: ${document.getElementById('equity').value} ש"ח`, 10, 60);
            doc.text(`דירוג אשראי: ${document.getElementById('credit-score').value}`, 10, 70);
            doc.text(`חובות קיימים: ${document.getElementById('existing-debts').value} ש"ח`, 10, 80);
            doc.text(`סכום הלוואה מבוקש: ${document.getElementById('loan-amount').value} ש"ח`, 10, 90);
            doc.text(`מטרת האשראי: ${document.getElementById('loan-purpose').value}`, 10, 100);
            doc.text(`פריסת הלוואה: ${document.getElementById('loan-term').value} שנים`, 10, 110);
            doc.text(`הסבר על מטרת האשראי: ${document.getElementById('loan-description').value}`, 10, 120);

            // הורדת ה-PDF
            doc.save("credit_request_report.pdf");
        }
    </script>
</body>
</html>
