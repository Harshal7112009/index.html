<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Secure Web ATM</title>
    <style>
        body { font-family: Arial, sans-serif; background-color: #121212; color: #ffffff; text-align: center; padding: 50px; }
        .atm-card { background: #1e1e1e; max-width: 400px; margin: auto; padding: 30px; border-radius: 15px; box-shadow: 0 4px 15px rgba(0,0,0,0.5); }
        input, select { width: 80%; padding: 10px; margin: 15px 0; border: none; border-radius: 5px; font-size: 16px; background-color: #2a2a2a; color: white; }
        button { background: #28a745; color: white; border: none; padding: 10px 20px; font-size: 16px; border-radius: 5px; cursor: pointer; font-weight: bold; }
        button:hover { background: #218838; }
        .hidden { display: none; }
    </style>
</head>
<body>

<div class="atm-card">
    <h2>=== SECURE BANK ATM ===</h2>

        <!-- STEP 1: PIN ENTRY -->
    <div id="step1">
        <p>Please enter your secret 4-digit PIN:</p>
        <input type="password" id="pinInput" placeholder="xxxx" maxlength="4">
        <br>
        <button onclick="verifyPin()">Submit PIN</button>
    </div>

    <!-- STEP 2: ACCOUNT TYPE -->
    <div id="step2" class="hidden">
        <p style="color: #28a745;">[SUCCESS] Password Verified.</p>
        <p>Choose Account Type:</p>
        <select id="accType">
            <option value="Savings">Savings Account</option>
            <option value="Current">Current Account</option>
        </select>
        <br>
        <button onclick="selectAccount()">Proceed</button>
    </div>

    <!-- STEP 3: TRANSACTION MENU -->
    <div id="step3" class="hidden">
        <p id="welcomeMsg" style="font-weight: bold; color: #17a2b8;"></p>
        <h3>Available Balance: Rs. <span id="balDisplay">25000.00</span></h3>
        <hr style="border-color: #333;">
        
        <p>Select Action:</p>
        <select id="actionSelect" onchange="handleActionChange()">
            <option value="check">Check Balance</option>
            <option value="deposit">Deposit Money</option>
            <option value="withdraw">Withdraw Money</option>
        </select>
        
        <div id="amountSection" class="hidden">
            <input type="number" id="amountInput" placeholder="Enter Amount (Rs.)">
        </div>
        <br>
        <button onclick="processTransaction()">Submit Transaction</button>
        <br><br>
        <p id="statusLog" style="color: #ffc107; font-weight: bold;"></p>
    </div>

</div>

<script>
    let balance = 25000.00;
    const correctPin = "0711"; // Tell your friend this password!

    function verifyPin() {
        let pin = document.getElementById("pinInput").value;
        // FIXED: Lowercase 'if' statement ensures browser execution handles string validation perfectly
        if (pin === correctPin) {
            document.getElementById("step1").classList.add("hidden");
            document.getElementById("step2").classList.remove("hidden");
        } else {
            alert("Incorrect PIN! Access Denied.");
        }
    }

    function selectAccount() {
        let type = document.getElementById("accType").value;
        document.getElementById("welcomeMsg").innerText = "Accessing: " + type;
        document.getElementById("step2").classList.add("hidden");
        document.getElementById("step3").classList.remove("hidden");
    }

    function handleActionChange() {
        let action = document.getElementById("actionSelect").value;
        if (action === "deposit" || action === "withdraw") {
            document.getElementById("amountSection").classList.remove("hidden");
        } else {
            document.getElementById("amountSection").classList.add("hidden");
        }
    }

    function processTransaction() {
        let action = document.getElementById("actionSelect").value;
        let amt = parseFloat(document.getElementById("amountInput").value) || 0;
        let log = document.getElementById("statusLog");

        if (action === "check") {
            log.innerText = "Inquiry: Your balance is securely loaded above.";
        } else if (action === "deposit") {
            if (amt > 0) {
                balance += amt;
                document.getElementById("balDisplay").innerText = balance.toFixed(2);
                log.innerText = "Successfully deposited Rs. " + amt;
            } else { 
                log.innerText = "Error: Deposit amount must be positive."; 
            }
        } else if (action === "withdraw") {
            if (amt > balance) { 
                log.innerText = "Error: Insufficient balance!"; 
            } else if (amt <= 0) { 
                log.innerText = "Error: Withdrawal amount must be positive."; 
            } else {
                balance -= amt;
                document.getElementById("balDisplay").innerText = balance.toFixed(2);
                log.innerText = "Please collect your cash: Rs. " + amt;
            }
        }
        document.getElementById("amountInput").value = "";
    }
</script>
</body>
</html>
