<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cookie Clicker</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background-color: #333;
            color: white;
            margin: 0;
            padding: 0;
        }
        #cookie {
            width: 150px;
            height: 150px;
            background-color: #f4a261;
            border-radius: 50%;
            display: inline-block;
            cursor: pointer;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
            margin-top: 100px;
            position: relative;
        }
        #score {
            font-size: 30px;
            font-weight: bold;
            margin-top: 20px;
        }
        #score span {
            font-size: 50px;
            color: #2d6a4f;
        }
        button, input {
            margin-top: 20px;
            padding: 10px 20px;
            font-size: 16px;
            background-color: #2d6a4f;
            color: white;
            border: none;
            cursor: pointer;
            border-radius: 5px;
        }
        button:hover, input:hover {
            background-color: #264c3f;
        }
        #passwordWall {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            flex-direction: column;
            text-align: center;
        }
        #passwordInput, #cookieAmountInput, #usernameInput {
            background-color: #444;
            border: 1px solid #555;
            color: white;
            border-radius: 5px;
            margin-top: 15px;
        }
        #gamePage {
            display: none;
        }
        .floating-text {
            position: absolute;
            font-size: 24px;
            font-weight: bold;
            color: #FFD700;
            opacity: 1;
            pointer-events: none;
            transition: all 0.8s ease-out;
        }
        .container {
            max-width: 800px;
            margin: 0 auto;
        }
        h1, h2 {
            color: #FFD700;
        }
        #userCount {
            margin-top: 20px;
        }
        #bgShopButtons {
            display: flex;
            justify-content: space-around;
            margin-top: 20px;
        }
        .bg-btn {
            width: 200px;
            height: 50px;
            margin: 0 10px;
            background-color: #00000;
            color: white;
            cursor: pointer;
            border: none;
            border-radius: 5px;
        }
        .bg-btn:disabled {
            background-color: #888;
            cursor: not-allowed;
        }
    </style>
</head>
<body>
    <div id="passwordWall">
        <h1>Enter Username and Password to Access the Game</h1>
        <input type="text" id="usernameInput" placeholder="Enter Username">
        <input type="password" id="passwordInput" placeholder="Enter password">
        <button id="submitPasswordBtn">Login</button>
        <p id="passwordError" style="color: red; display: none;">Incorrect Password! Please try again.</p>
        <h3 id="userCount">Current Users: 0</h3>
    </div>

    <div id="gamePage">
        <div class="container">
            <h2 id="usernameDisplay">Welcome, Player!</h2>
            <div id="cookie"></div>
            <div id="score">Cookies: <span>0</span></div>
            <input type="password" id="passwordInputForExtras" placeholder="Code">
            <button id="upgrade">Upgrade - Cost: 50</button>
            <button id="doubleButton" style="display: none;">2x - Double Cookies</button>
            <input type="number" id="cookieAmountInput" style="display: none;" placeholder="Enter custom cookies amount" max="9999">
            <button id="addCookiesBtn" style="display: none;">Add Cookies</button>

            <!-- Background Shop -->
            <div id="bgShopButtons">
                <button id="bg1Btn" class="bg-btn">(100 Cookies)</button>
                <button id="bg2Btn" class="bg-btn">(200 Cookies)</button>
                <button id="bg3Btn" class="bg-btn">(300 Cookies)</button>
            </div>

            <!-- Logout Button -->
            <button id="logoutBtn">Logout</button>
        </div>
    </div>

    <script>
        let cookies = 0;
        let cookiesPerClick = 1;
        let upgradeCost = 50;
        let upgradeCount = 0;
        let cookieInterval;
        const cookieElement = document.getElementById('cookie');
        const scoreElement = document.getElementById('score').children[0];
        const upgradeButton = document.getElementById('upgrade');
        const doubleButton = document.getElementById('doubleButton');
        const cookieAmountInput = document.getElementById('cookieAmountInput');
        const addCookiesBtn = document.getElementById('addCookiesBtn');
        const passwordInputForExtras = document.getElementById('passwordInputForExtras');
        const passwordWall = document.getElementById('passwordWall');
        const gamePage = document.getElementById('gamePage');
        const submitPasswordBtn = document.getElementById('submitPasswordBtn');
        const passwordError = document.getElementById('passwordError');
        const usernameInput = document.getElementById('usernameInput');
        const usernameDisplay = document.getElementById('usernameDisplay');
        const userCountElement = document.getElementById('userCount');
        const logoutButton = document.getElementById('logoutBtn');
        let username = '';

        if (!localStorage.getItem('userCount')) {
            localStorage.setItem('userCount', '0');
        }

        function updateUserCount() {
            const currentCount = parseInt(localStorage.getItem('userCount'));
            userCountElement.textContent = `Current Users: ${currentCount}`;
        }

        updateUserCount();

        cookieElement.addEventListener('click', function(event) {
            cookies += cookiesPerClick;
            scoreElement.textContent = cookies;
        });

        upgradeButton.addEventListener('click', function() {
            if (cookies >= upgradeCost) {
                cookies -= upgradeCost;
                cookiesPerClick += 1;
                upgradeCount += 1;
                upgradeCost = Math.floor(upgradeCost * 1.5);
                scoreElement.textContent = cookies;
                upgradeButton.textContent = `Upgrade - Cost: ${upgradeCost}`;
            } else {
                alert("You don't have enough cookies for this upgrade!");
            }
        });

        submitPasswordBtn.addEventListener('click', function() {
            const enteredPassword = document.getElementById('passwordInput').value;
            const enteredUsername = usernameInput.value.trim();

            if (enteredUsername === "Nigger" || enteredUsername === "Mrbeast" || enteredUsername === "Owner" || enteredUsername === "Admin" || enteredUsername === "Dick" || enteredUsername === "Faggot" || enteredUsername === "Bitch") {
                alert("This username is not allowed.");
                return;
            }

            if (enteredPassword === 'Akuma') {
                username = enteredUsername || 'Player';
                usernameDisplay.innerHTML = `<span style="color: Green;">[User]</span> ${username}`;
                passwordWall.style.display = 'none';
                gamePage.style.display = 'block';

                let currentCount = parseInt(localStorage.getItem('userCount'));
                localStorage.setItem('userCount', (currentCount + 1).toString());
                updateUserCount();

                cookieInterval = setInterval(function() {
                    cookies += 1;
                    scoreElement.textContent = cookies;
                }, 1000);

            } else if (enteredPassword === 'kingakuma') {
                username = enteredUsername || 'Player';
                usernameDisplay.innerHTML = `<span style="color: Blue;">[Owner]</span> ${username}`;
                passwordWall.style.display = 'none';
                gamePage.style.display = 'block';

                let currentCount = parseInt(localStorage.getItem('userCount'));
                localStorage.setItem('userCount', (currentCount + 1).toString());
                updateUserCount();

                cookieInterval = setInterval(function() {
                    cookies += 1;
                    scoreElement.textContent = cookies;
                }, 1000);

            } else {
                passwordError.style.display = 'block';
            }
        });

        passwordInputForExtras.addEventListener('input', function() {
            const enteredPassword = passwordInputForExtras.value;
            if (enteredPassword === 'kingakuma') {
                usernameDisplay.innerHTML = `<span style="color: Blue;">[Owner]</span> ${username}`;
                cookieAmountInput.style.display = 'inline-block'; // Show custom cookie amount input
                addCookiesBtn.style.display = 'inline-block';   // Show add cookies button
                doubleButton.style.display = 'inline-block'; // Show 2x button
            } else {
                usernameDisplay.textContent = `Welcome, ${usernameDisplay.textContent.split(' ')[1]}`;
                cookieAmountInput.style.display = 'none';       // Hide custom cookie amount input
                addCookiesBtn.style.display = 'none';           // Hide add cookies button
                doubleButton.style.display = 'none'; // Hide 2x button
            }
        });

        doubleButton.addEventListener('click', function() {
            cookies *= 2;
            scoreElement.textContent = cookies;
        });

        addCookiesBtn.addEventListener('click', function() {
            const customAmount = parseInt(cookieAmountInput.value);
            if (!isNaN(customAmount) && customAmount > 0 && customAmount <= 9999) {
                cookies += customAmount;
                scoreElement.textContent = cookies;
            } else {
                alert("Max is 9,999 set by the dev. soz");
                cookieAmountInput.value = '';
            }
        });

        logoutButton.addEventListener('click', function() {
            // Reset cookies and hide the game page
            clearInterval(cookieInterval);
            cookies = 0;
            scoreElement.textContent = cookies;
            usernameDisplay.textContent = 'Welcome, Player!';
            gamePage.style.display = 'none';
            passwordWall.style.display = 'flex'; // Show login page
        });

        document.getElementById('bg1Btn').addEventListener('click', function() {
            if (cookies >= 100) {
                cookies -= 100;
                document.body.style.backgroundImage = "url('https://c.tenor.com/otkcxxtw3vUAAAAd/tenor.gif')";
                scoreElement.textContent = cookies;
                document.getElementById('bg1Btn').disabled = true;
            } else {
                alert("You don't have enough cookies!");
            }
        });

        document.getElementById('bg2Btn').addEventListener('click', function() {
            if (cookies >= 200) {
                cookies -= 200;
                document.body.style.backgroundImage = "url('https://images7.alphacoders.com/108/1081933.jpg')";
                scoreElement.textContent = cookies;
                document.getElementById('bg2Btn').disabled = true;
            } else {
                alert("You don't have enough cookies!");
            }
        });

        document.getElementById('bg3Btn').addEventListener('click', function() {
            if (cookies >= 300) {
                cookies -= 300;
                document.body.style.backgroundImage = "url('https://images.pexels.com/photos/2387793/pexels-photo-2387793.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1')";
                scoreElement.textContent = cookies;
                document.getElementById('bg3Btn').disabled = true;
            } else {
                alert("You don't have enough cookies!");
            }
        });
    </script>
</body>
</html>
