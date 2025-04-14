<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>USA vs China Clicker War</title>
    <style>
    /* --- CSS Start --- */
    body {
        font-family: sans-serif;
        text-align: center;
        background-color: #f0f0f0;
        color: #333;
        padding: 10px; /* Slightly reduce padding for smaller screens */
        margin: 0; /* Ensure no default body margin */
        -webkit-text-size-adjust: 100%; /* Prevent font scaling on iOS */
    }

    h1 {
        color: #1a1a1a;
        font-size: 1.8em; /* Relative font size */
    }

    .game-container {
        display: flex;
        flex-wrap: wrap; /* Allow wrapping if needed, useful with flex-direction change */
        justify-content: space-around;
        align-items: flex-start;
        margin: 20px auto; /* Reduced top/bottom margin slightly */
        /* Responsive width: takes 95% of screen, up to 800px */
        width: 95%;
        max-width: 800px;
        background-color: #fff;
        padding: 15px; /* Reduced padding slightly */
        border-radius: 8px;
        box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    }

    .country-section {
        text-align: center;
        padding: 15px;
        border: 1px solid #ddd;
        border-radius: 5px;
        background-color: #f9f9f9;
        /* Takes up roughly half the container width, allows shrinking */
        flex-basis: 45%;
        min-width: 250px; /* Prevent sections from becoming too narrow before wrapping/stacking */
        margin-bottom: 15px; /* Add space below when stacking */
    }

    .flag-container {
        /* font-size removed as we use images */
        margin: 15px 0; /* Reduced margin */
        transition: transform 0.1s ease;
        user-select: none;
        -webkit-user-select: none;
        -ms-user-select: none;
    }

    /* --- This rule sizes the flag images --- */
    .flag-container img {
        /* Responsive image: 60% of its container, but no wider than 150px */
        width: 60%;
        max-width: 150px;
        height: auto; /* Maintain aspect ratio */
        display: block;
        margin-left: auto;
        margin-right: auto;
        /* border: 1px solid #ccc; */ /* Optional border */
    }

    .flag-container.clickable {
        cursor: pointer;
    }

    .flag-container.clickable:active {
        transform: scale(0.95);
    }

    .country-section h2 {
        margin-bottom: 10px; /* Reduced margin */
        color: #555;
        font-size: 1.5em;
    }

    .country-section p {
        font-size: 1em; /* Adjusted base font size */
        margin: 6px 0; /* Reduced margin */
    }

    .country-section span {
        font-weight: bold;
    }

    /* Style specific country stats differently */
    #usa-section span#usa-score, #usa-section span#usa-tariff {
         color: #0052cc; /* USA Blue */
    }
    #china-section span#china-score, #china-section span#china-tariff {
         color: #de2910; /* China Red */
    }

    .leaderboard {
        margin: 30px auto; /* Adjusted margin */
        background-color: #e9ecef;
        padding: 15px; /* Reduced padding */
        border-radius: 8px;
        /* Responsive width: takes 95% of screen, up to 600px */
        width: 95%;
        max-width: 600px;
        margin-left: auto;
        margin-right: auto;
        box-shadow: 0 1px 3px rgba(0,0,0,0.1);
    }

    .leaderboard h2 {
        margin-bottom: 10px; /* Reduced margin */
        font-size: 1.3em;
    }

    .leaderboard p {
        font-size: 1em; /* Adjusted base font size */
        line-height: 1.5; /* Adjusted line height */
    }

    .leaderboard span {
        font-weight: bold;
    }

    #leading-country {
        color: green; /* Default leading color */
    }

    /* --- Media Query for Smaller Screens (e.g., phones) --- */
    @media (max-width: 600px) {
        body {
            padding: 5px; /* Even less padding on very small screens */
        }

        h1 {
            font-size: 1.5em; /* Smaller H1 on small screens */
        }

        .game-container {
            flex-direction: column; /* Stack country sections vertically */
            align-items: center; /* Center items when stacked */
            padding: 10px;
        }

        .country-section {
            flex-basis: 90%; /* Allow sections to take up more width when stacked */
            width: 90%; /* Ensure width is applied */
            min-width: 0; /* Reset min-width */
            margin-bottom: 20px; /* Ensure spacing between stacked items */
        }

        /* Optional: Make flags slightly smaller on very small screens */
        .flag-container img {
             max-width: 120px; /* Smaller max width for flags */
        }

        .country-section h2 {
            font-size: 1.3em;
        }

         .leaderboard h2 {
            font-size: 1.2em;
        }

        .country-section p, .leaderboard p {
            font-size: 0.9em; /* Slightly smaller text */
        }
    }

    /* --- CSS End --- */
</style>
</head>
<body>

    <h1>USA vs China Clicker War</h1>
    <p>Click the flag to increase your country's score and tariffs!</p>

    <div class="game-container">
        <div class="country-section" id="usa-section">
            <h2>USA</h2>
            <div class="flag-container clickable" id="usa-flag">
                <img src="usa-flag.jpg" alt="USA Flag">
                </div>
            <p>Clicks: <span id="usa-score">0</span></p>
            <p>Tariffs: <span id="usa-tariff">0</span>%</p>
        </div>

        <div class="country-section" id="china-section">
            <h2>China</h2>
            <div class="flag-container clickable" id="china-flag">
                <img src="china-flag.jpg" alt="China Flag">
                 </div>
            <p>Clicks: <span id="china-score">0</span></p>
            <p>Tariffs: <span id="china-tariff">0</span>%</p>
        </div>
    </div>

    <div class="leaderboard">
        <h2>Leaderboard</h2>
        <p>USA Total Clicks: <span id="leaderboard-usa">0</span> (<span id="leaderboard-usa-tariff">0</span>% Tariffs)</p>
        <p>China Total Clicks: <span id="leaderboard-china">0</span> (<span id="leaderboard-china-tariff">0</span>% Tariffs)</p>
        <p><strong>Leading: <span id="leading-country">Neither</span></strong></p>
    </div>

    <script>
        // --- JavaScript Start ---

        // --- DOM Elements ---
        const usaFlag = document.getElementById('usa-flag');
        const chinaFlag = document.getElementById('china-flag');

        const usaScoreDisplay = document.getElementById('usa-score');
        const chinaScoreDisplay = document.getElementById('china-score');

        const usaTariffDisplay = document.getElementById('usa-tariff');
        const chinaTariffDisplay = document.getElementById('china-tariff');

        const leaderboardUsa = document.getElementById('leaderboard-usa');
        const leaderboardChina = document.getElementById('leaderboard-china');
        const leaderboardUsaTariff = document.getElementById('leaderboard-usa-tariff');
        const leaderboardChinaTariff = document.getElementById('leaderboard-china-tariff');
        const leadingCountryDisplay = document.getElementById('leading-country');

        // --- Game State ---
        // Try to load scores from localStorage, default to 0 if not found
        let usaScore = parseInt(localStorage.getItem('usaScore')) || 0;
        let chinaScore = parseInt(localStorage.getItem('chinaScore')) || 0;

        // --- Functions ---

        // Function to calculate tariff percentage (simple example: square root based, capped at 100%)
        function calculateTariff(score) {
            if (score <= 0) return 0;
            // Example formula: tariff grows but slows down. Adjust multiplier (0.5) as needed.
            // Math.min ensures it doesn't go above 100%
            const tariff = Math.min(100, Math.floor(Math.sqrt(score) * 0.5));
            return tariff;
        }

        // Function to update all displays and save scores
        function updateDisplay() {
            // Calculate tariffs
            const usaTariff = calculateTariff(usaScore);
            const chinaTariff = calculateTariff(chinaScore);

            // Update individual country displays
            // Format with commas for readability using toLocaleString()
            usaScoreDisplay.textContent = usaScore.toLocaleString();
            usaTariffDisplay.textContent = usaTariff;
            chinaScoreDisplay.textContent = chinaScore.toLocaleString();
            chinaTariffDisplay.textContent = chinaTariff;

            // Update leaderboard displays
            leaderboardUsa.textContent = usaScore.toLocaleString();
            leaderboardChina.textContent = chinaScore.toLocaleString();
            leaderboardUsaTariff.textContent = usaTariff;
            leaderboardChinaTariff.textContent = chinaTariff;

            // Update leading country display and color
            if (usaScore > chinaScore) {
                leadingCountryDisplay.textContent = 'USA';
                leadingCountryDisplay.style.color = '#0052cc'; // USA Blue
            } else if (chinaScore > usaScore) {
                leadingCountryDisplay.textContent = 'China';
                 leadingCountryDisplay.style.color = '#de2910'; // China Red
            } else {
                // Handle the tie case
                leadingCountryDisplay.textContent = 'Neither (Tie)';
                leadingCountryDisplay.style.color = 'grey'; // Neutral color for tie
            }

            // Save scores to localStorage for persistence
            localStorage.setItem('usaScore', usaScore);
            localStorage.setItem('chinaScore', chinaScore);
        }

        // --- Event Listeners ---

        usaFlag.addEventListener('click', () => {
            usaScore++; // Increment score
            updateDisplay(); // Update the UI and save
        });

        chinaFlag.addEventListener('click', () => {
            chinaScore++; // Increment score
            updateDisplay(); // Update the UI and save
        });

        // --- Initial Setup ---
        // Call updateDisplay once on page load to show initial scores (from localStorage or 0)
        updateDisplay();

        // --- JavaScript End ---
    </script>

</body>
</html>
