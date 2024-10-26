# gagyebuu
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>가계부 애플리케이션</title>
    <script type="module">
        // Import Firebase modules
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.0.1/firebase-app.js";
        import { getDatabase, ref, set, onValue } from "https://www.gstatic.com/firebasejs/11.0.1/firebase-database.js";

        // Your web app's Firebase configuration
        const firebaseConfig = {
            apiKey: "AIzaSyA5WjUlY4rAF0cjeYotz_MntTgpy_h1EhY",
            authDomain: "gagyebu2-b5c61.firebaseapp.com",
            projectId: "gagyebu2-b5c61",
            storageBucket: "gagyebu2-b5c61.appspot.com",
            messagingSenderId: "845633990000",
            appId: "1:845633990000:web:cfaacfeb4eb00d83cbb3b5",
            measurementId: "G-L3RLCR8V69"
        };

        // Initialize Firebase
        const app = initializeApp(firebaseConfig);
        const database = getDatabase(app);

        let totalIncome = 0;
        let totalExpense = 0;

        function saveTransaction(type, amount, date) {
            const transactionRef = ref(database, 'transactions/' + Date.now());
            set(transactionRef, {
                type: type,
                amount: amount,
                date: date
            });
            updateTransactionList(type, amount, date); // Update the transaction list
        }

        function updateTransactionList(type, amount, date) {
            const transactionsDiv = document.getElementById("transactions");
            const transactionEntry = document.createElement("div");
            transactionEntry.innerText = `${date}: ${type} - ${amount} 원`;
            transactionsDiv.appendChild(transactionEntry);
        }

        function updateBalance() {
            const balance = totalIncome - totalExpense;
            document.getElementById("balance").innerText = `현재 잔액: ${balance} 원`;
        }

        document.addEventListener('DOMContentLoaded', function() {
            document.getElementById("addIncome").addEventListener("click", function() {
                const amount = parseFloat(document.getElementById("income").value) || 0;
                const date = new Date().toLocaleDateString();
                if (amount) {
                    totalIncome += amount; // Update total income
                    saveTransaction("수입", amount, date);
                    updateBalance();
                    document.getElementById("income").value = ""; // Clear input
                }
            });

            document.getElementById("addExpense").addEventListener("click", function() {
                const amount = parseFloat(document.getElementById("expense").value) || 0;
                const date = new Date().toLocaleDateString();
                if (amount) {
                    totalExpense += amount; // Update total expense
                    saveTransaction("지출", amount, date);
                    updateBalance();
                    document.getElementById("expense").value = ""; // Clear input
                }
            });
        });
    </script>
    <style>
        body {
            background-color: black;
            color: white;
            font-family: Arial, sans-serif;
            text-align: center;
            padding: 20px;
        }
        input {
            margin: 10px;
            padding: 10px;
            width: 200px;
        }
        button {
            padding: 10px 20px;
            margin: 10px;
            cursor: pointer;
        }
        #balance {
            font-size: 24px;
            margin-top: 20px;
        }
        #transactions {
            margin-top: 20px;
            text-align: left;
        }
    </style>
</head>
<body>
    <h1>가계부 애플리케이션</h1>

    <div>
        <input type="number" id="income" placeholder="수입 금액" />
        <button id="addIncome">수입 추가</button>
    </div>

    <div>
        <input type="number" id="expense" placeholder="지출 금액" />
        <button id="addExpense">지출 추가</button>
    </div>

    <div id="balance">현재 잔액: 0 원</div>

    <div id="transactions"></div>
</body>
</html>
