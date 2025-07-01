<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Life Choices Game</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background: linear-gradient(135deg, #f0f9ff 0%, #c9e8ff 100%); /* Light blue gradient background */
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }
        .game-container {
            background-color: #ffffff;
            border-radius: 20px;
            box-shadow: 0 15px 30px rgba(0, 0, 0, 0.15);
            padding: 30px;
            max-width: 600px;
            width: 100%;
            text-align: center;
            display: flex;
            flex-direction: column;
            gap: 20px;
            border: 2px solid #e0f2fe; /* Light blue border */
        }
        .stats-panel {
            display: flex;
            justify-content: space-around;
            gap: 10px;
            margin-bottom: 20px;
        }
        .stat-item {
            background-color: #e0f7fa; /* Light cyan background */
            padding: 15px 20px;
            border-radius: 15px;
            box-shadow: 0 5px 10px rgba(0, 0, 0, 0.08);
            flex: 1;
            font-weight: 600;
            color: #00796b; /* Dark cyan text */
            transition: transform 0.2s ease-in-out;
        }
        .stat-item:hover {
            transform: translateY(-3px);
        }
        .scenario-text {
            font-size: 1.35rem;
            color: #334155; /* Slate 700 */
            margin-bottom: 25px;
            line-height: 1.6;
            font-weight: 500;
        }
        .options-container {
            display: flex;
            flex-direction: column;
            gap: 15px;
        }
        .choice-button {
            background: linear-gradient(135deg, #60a5fa 0%, #3b82f6 100%); /* Blue gradient */
            color: white;
            padding: 15px 25px;
            border-radius: 15px;
            font-size: 1.1rem;
            font-weight: 600;
            cursor: pointer;
            border: none;
            box-shadow: 0 8px 15px rgba(0, 0, 0, 0.1);
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
            letter-spacing: 0.5px;
        }
        .choice-button:hover {
            transform: translateY(-5px);
            box-shadow: 0 12px 20px rgba(0, 0, 0, 0.2);
            background: linear-gradient(135deg, #3b82f6 0%, #2563eb 100%); /* Darker blue gradient */
        }
        .choice-button:active {
            transform: translateY(-2px);
            box-shadow: 0 5px 10px rgba(0, 0, 0, 0.1);
        }
        .choice-button::before {
            content: '';
            position: absolute;
            top: 50%;
            left: 50%;
            width: 300%;
            height: 300%;
            background: rgba(255, 255, 255, 0.15);
            border-radius: 50%;
            transition: all 0.75s ease-out;
            transform: translate(-50%, -50%) scale(0);
            opacity: 0;
        }
        .choice-button:hover::before {
            transform: translate(-50%, -50%) scale(1);
            opacity: 1;
        }
        .message-box {
            background-color: #fff3e0; /* Light orange background */
            color: #e65100; /* Dark orange text */
            padding: 15px;
            border-radius: 15px;
            margin-top: 20px;
            font-size: 1rem;
            font-weight: 500;
            box-shadow: 0 5px 10px rgba(0, 0, 0, 0.08);
            min-height: 50px; /* Ensure it has some height */
            display: flex;
            align-items: center;
            justify-content: center;
            text-align: center;
        }
        .game-over-screen {
            background-color: #ffebee; /* Light red background */
            color: #c62828; /* Dark red text */
            padding: 30px;
            border-radius: 20px;
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
            margin-top: 20px;
            display: none; /* Hidden by default */
            flex-direction: column;
            gap: 20px;
            font-size: 1.2rem;
            font-weight: 600;
        }
        .restart-button {
            background: linear-gradient(135deg, #ef4444 0%, #dc2626 100%); /* Red gradient */
            color: white;
            padding: 15px 30px;
            border-radius: 15px;
            font-size: 1.1rem;
            font-weight: 700;
            cursor: pointer;
            border: none;
            box-shadow: 0 8px 15px rgba(0, 0, 0, 0.1);
            transition: all 0.3s ease;
        }
        .restart-button:hover {
            transform: translateY(-5px);
            box-shadow: 0 12px 20px rgba(0, 0, 0, 0.2);
            background: linear-gradient(135deg, #dc2626 0%, #b91c1c 100%); /* Darker red gradient */
        }
        .restart-button:active {
            transform: translateY(-2px);
            box-shadow: 0 5px 10px rgba(0, 0, 0, 0.1);
        }
    </style>
</head>
<body>

    <div class="game-container">
        <h1 class="text-4xl font-bold text-gray-800 mb-4">Life Choices</h1>

        <div class="stats-panel">
            <div id="money-stat" class="stat-item">Money: $1000</div>
            <div id="happiness-stat" class="stat-item">Happiness: 100</div>
        </div>

        <div id="scenario-display" class="scenario-text">
            Welcome to the Life Choices Game! Make decisions that affect your future.
        </div>

        <div id="options-container" class="options-container">
            <!-- Choices will be dynamically inserted here -->
            <button class="choice-button" onclick="startGame()">Start Game</button>
        </div>

        <div id="message-box" class="message-box">
            Good luck!
        </div>

        <div id="game-over-screen" class="game-over-screen">
            <p id="game-over-message"></p>
            <button class="restart-button" onclick="restartGame()">Restart Game</button>
        </div>
    </div>

    <script>
        // Game state variables
        let money = 0;
        let happiness = 0;
        let currentScenarioIndex = 0;
        let gameOver = false;

        // DOM elements
        const moneyStat = document.getElementById('money-stat');
        const happinessStat = document.getElementById('happiness-stat');
        const scenarioDisplay = document.getElementById('scenario-display');
        const optionsContainer = document.getElementById('options-container');
        const messageBox = document.getElementById('message-box');
        const gameOverScreen = document.getElementById('game-over-screen');
        const gameOverMessage = document.getElementById('game-over-message');

        // Game scenarios
        const scenarios = [
            {
                text: "You wake up to a new day. What's your first move?",
                options: [
                    {
                        text: "Go to work early and get a head start.",
                        moneyChange: 100,
                        happinessChange: -10,
                        consequence: "You earned extra cash but felt a bit tired.",
                        isGameOver: false
                    },
                    {
                        text: "Sleep in and enjoy a relaxed morning.",
                        moneyChange: -50,
                        happinessChange: 20,
                        consequence: "You lost some potential earnings but feel refreshed.",
                        isGameOver: false
                    }
                ]
            },
            {
                text: "Your old car breaks down. What do you do?",
                options: [
                    {
                        text: "Buy a brand new, expensive car.",
                        moneyChange: -500,
                        happinessChange: 30,
                        consequence: "You're happy with your new ride, but your wallet is much lighter.",
                        isGameOver: false
                    },
                    {
                        text: "Fix the old car yourself, cheaply.",
                        moneyChange: -100,
                        happinessChange: -5,
                        consequence: "It was a hassle, but you saved a lot of money.",
                        isGameOver: false
                    },
                    {
                        text: "Take public transport for a while.",
                        moneyChange: 50,
                        happinessChange: -20,
                        consequence: "You saved money, but commuting is a drag.",
                        isGameOver: false
                    }
                ]
            },
            {
                text: "A friend asks for a large loan. What's your decision?",
                options: [
                    {
                        text: "Lend them the money, no questions asked.",
                        moneyChange: -300,
                        happinessChange: 40,
                        consequence: "You're a good friend, but you might not see that money again.",
                        isGameOver: false
                    },
                    {
                        text: "Decline politely, citing your own financial needs.",
                        moneyChange: 0,
                        happinessChange: -15,
                        consequence: "You kept your money, but your friend is a little disappointed.",
                        isGameOver: false
                    },
                    {
                        text: "Lend them half, with a clear repayment plan.",
                        moneyChange: -150,
                        happinessChange: 10,
                        consequence: "You helped them, but also protected yourself.",
                        isGameOver: false
                    }
                ]
            },
            {
                text: "You find a winning lottery ticket worth a significant amount. What do you do?",
                options: [
                    {
                        text: "Claim the full amount and keep it secret.",
                        moneyChange: 2000,
                        happinessChange: 50,
                        consequence: "You're rich! But the secret weighs on you slightly.",
                        isGameOver: false
                    },
                    {
                        text: "Donate a large portion to charity.",
                        moneyChange: 1000,
                        happinessChange: 70,
                        consequence: "You made a huge positive impact and feel great about it.",
                        isGameOver: false
                    },
                    {
                        text: "Try to find the rightful owner.",
                        moneyChange: 0,
                        happinessChange: 100,
                        consequence: "You did the right thing, even if it meant no personal gain.",
                        isGameOver: false
                    }
                ]
            },
            {
                text: "You're offered a dream job, but it requires moving to a new city far away from family and friends. Do you take it?",
                options: [
                    {
                        text: "Accept the job, embrace the new adventure.",
                        moneyChange: 700,
                        happinessChange: 30,
                        consequence: "You're on a great career path, but miss your loved ones.",
                        isGameOver: false
                    },
                    {
                        text: "Decline the job to stay close to your support system.",
                        moneyChange: -200,
                        happinessChange: 60,
                        consequence: "You sacrificed career growth for personal comfort. You feel content.",
                        isGameOver: false
                    }
                ]
            },
            {
                text: "You've been neglecting your health. What's your next step?",
                options: [
                    {
                        text: "Join a gym and commit to a healthy lifestyle.",
                        moneyChange: -50,
                        happinessChange: 40,
                        consequence: "It costs money, but you feel much better physically and mentally.",
                        isGameOver: false
                    },
                    {
                        text: "Ignore it, hoping it will get better on its own.",
                        moneyChange: 0,
                        happinessChange: -30,
                        consequence: "Your health deteriorates, affecting your mood and energy.",
                        isGameOver: false
                    }
                ]
            },
            {
                text: "You have a chance to invest in a risky but potentially very profitable startup. Do you invest?",
                options: [
                    {
                        text: "Go all in, hoping for a massive return.",
                        moneyChange: -400,
                        happinessChange: 50,
                        consequence: "High risk, high reward! You're either rich or broke. (This could lead to game over if it fails!)",
                        isGameOver: false // This will be handled dynamically based on outcome
                    },
                    {
                        text: "Invest a small, manageable amount.",
                        moneyChange: -100,
                        happinessChange: 10,
                        consequence: "A safer bet. You're cautiously optimistic.",
                        isGameOver: false
                    },
                    {
                        text: "Don't invest at all, play it safe.",
                        moneyChange: 0,
                        happinessChange: 5,
                        consequence: "No risk, no reward. You feel secure.",
                        isGameOver: false
                    }
                ]
            }
        ];

        // Function to update the display of money and happiness
        function updateStats() {
            moneyStat.textContent = `Money: $${money}`;
            happinessStat.textContent = `Happiness: ${happiness}`;
        }

        // Function to display messages
        function displayMessage(message, type = 'info') {
            messageBox.textContent = message;
            // Reset classes
            messageBox.className = 'message-box';
            if (type === 'error') {
                messageBox.classList.add('bg-red-100', 'text-red-800');
                messageBox.classList.remove('bg-amber-100', 'text-amber-800'); // Remove info classes if present
            } else { // 'info' or default
                messageBox.classList.add('bg-amber-100', 'text-amber-800');
                messageBox.classList.remove('bg-red-100', 'text-red-800'); // Remove error classes if present
            }
        }

        // Function to check for game over conditions
        function checkGameOver() {
            if (money <= 0) {
                endGame("You ran out of money! Game Over.");
                return true;
            }
            if (happiness <= 0) {
                endGame("You lost all your happiness! Game Over.");
                return true;
            }
            return false;
        }

        // Function to end the game
        function endGame(message) {
            gameOver = true;
            scenarioDisplay.style.display = 'none';
            optionsContainer.style.display = 'none';
            messageBox.style.display = 'none';
            gameOverScreen.style.display = 'flex';
            gameOverMessage.textContent = message;
        }

        // Function to restart the game
        function restartGame() {
            money = 1000;
            happiness = 100;
            currentScenarioIndex = 0;
            gameOver = false;
            updateStats();
            displayMessage("Game restarted! Make your choices wisely.");
            gameOverScreen.style.display = 'none';
            scenarioDisplay.style.display = 'block';
            optionsContainer.style.display = 'flex';
            messageBox.style.display = 'flex'; // Ensure message box is visible again
            displayScenario();
        }

        // Function to display the current scenario
        function displayScenario() {
            if (gameOver) return;

            if (currentScenarioIndex >= scenarios.length) {
                endGame("Congratulations! You've completed all scenarios. You win!");
                return;
            }

            const scenario = scenarios[currentScenarioIndex];
            scenarioDisplay.textContent = scenario.text;
            optionsContainer.innerHTML = ''; // Clear previous options

            scenario.options.forEach((option, index) => {
                const button = document.createElement('button');
                button.className = 'choice-button';
                button.textContent = option.text;
                button.onclick = () => chooseOption(option);
                optionsContainer.appendChild(button);
            });
        }

        // Function to handle a chosen option
        function chooseOption(option) {
            if (gameOver) return;

            money += option.moneyChange;
            happiness += option.happinessChange;

            // Special handling for the risky investment scenario
            if (scenarioDisplay.textContent.includes("risky but potentially very profitable startup") && option.text.includes("Go all in")) {
                const outcome = Math.random(); // 0 to 1
                if (outcome < 0.3) { // 30% chance of failure
                    money -= 1000; // Lose more than invested
                    happiness -= 50;
                    displayMessage("Your risky investment failed! You lost a significant amount.", 'error');
                } else { // 70% chance of success
                    money += 2000; // Gain a lot
                    happiness += 70;
                    displayMessage("Your risky investment paid off handsomely!", 'info');
                }
            } else {
                displayMessage(option.consequence);
            }

            updateStats();

            if (checkGameOver()) {
                return;
            }

            currentScenarioIndex++;
            displayScenario();
        }

        // Initial game setup
        function startGame() {
            restartGame(); // Use restartGame to initialize everything
        }

        // Call startGame when the page loads to show the initial state
        window.onload = startGame;

    </script>
</body>
</html>
