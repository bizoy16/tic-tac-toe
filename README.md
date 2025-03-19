# tic-tac-toe
i build a basic front end





<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tic Tac Toe</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Arial', sans-serif;
        }

        body {
            background-color: #f5f5f5;
            position: relative;
            min-height: 100vh;
            overflow-x: hidden;
        }

        /* Background pattern */
        .bg-pattern {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -1;
            opacity: 0.05;
            pointer-events: none;
        }

        .bg-pattern::before, .bg-pattern::after {
            content: '';
            position: absolute;
            width: 100%;
            height: 100%;
        }

        .bg-pattern::before {
            background: 
                linear-gradient(#333 1px, transparent 1px) 0 0,
                linear-gradient(90deg, #333 1px, transparent 1px) 0 0;
            background-size: 100px 100px;
        }

        .bg-pattern::after {
            background: 
                linear-gradient(45deg, #3498db 25%, transparent 25%) 0 0,
                linear-gradient(-45deg, #3498db 25%, transparent 25%) 0 0,
                linear-gradient(45deg, transparent 75%, #3498db 75%) 0 0,
                linear-gradient(-45deg, transparent 75%, #3498db 75%) 0 0;
            background-size: 200px 200px;
            background-position: 0 0, 0 100px, 100px -100px, -100px 0px;
            opacity: 0.1;
        }

        header {
            background-color: #3498db;
            color: white;
            text-align: center;
            padding: 1rem;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
            position: relative;
            z-index: 10;
        }

        header h1 {
            font-size: 2rem;
            text-transform: uppercase;
            letter-spacing: 2px;
        }

        .main-screen, .game-screen, .how-to-play-screen, .settings-screen {
            padding: 2rem;
            max-width: 500px;
            margin: 0 auto;
            text-align: center;
            transition: all 0.3s ease;
        }

        .screen {
            display: none;
            opacity: 0;
            transform: translateY(20px);
            transition: opacity 0.3s ease, transform 0.3s ease;
        }

        .screen.active {
            display: block;
            opacity: 1;
            transform: translateY(0);
        }

        .welcome-message {
            margin: 2rem 0;
            font-size: 1.2rem;
            color: #333;
        }

        .btn {
            display: flex;
            align-items: center;
            justify-content: center;
            background-color: #3498db;
            color: white;
            border: none;
            padding: 0.8rem 1.5rem;
            margin: 1rem auto;
            border-radius: 5px;
            font-size: 1rem;
            cursor: pointer;
            width: 80%;
            max-width: 300px;
            transition: all 0.2s ease;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
            position: relative;
            overflow: hidden;
        }

        .btn:hover {
            background-color: #2980b9;
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }

        .btn:active {
            transform: scale(0.98);
        }

        .btn i {
            margin-right: 10px;
            font-size: 1.2rem;
        }

        /* Game Board */
        .game-board {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            grid-gap: 10px;
            margin: 2rem auto;
            max-width: 300px;
        }

        .cell {
            background-color: white;
            aspect-ratio: 1;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2.5rem;
            font-weight: bold;
            cursor: pointer;
            border-radius: 5px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
            transition: all 0.2s ease;
            position: relative;
            overflow: hidden;
        }

        .cell:hover {
            background-color: #f0f0f0;
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }

        .cell.x {
            color: #e74c3c;
        }

        .cell.o {
            color: #3498db;
        }

        .cell.winner {
            animation: pulse 1s infinite;
        }

        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }

        .game-info {
            margin: 1rem 0;
            font-size: 1.2rem;
        }

        .player-turn {
            display: inline-block;
            padding: 0.5rem 1rem;
            background-color: #f0f0f0;
            border-radius: 5px;
            font-weight: bold;
        }

        .player-turn.x {
            color: #e74c3c;
        }

        .player-turn.o {
            color: #3498db;
        }

        .result-message {
            font-size: 1.5rem;
            font-weight: bold;
            margin: 1rem 0;
            padding: 1rem;
            border-radius: 5px;
            display: none;
            animation: fadeIn 0.5s ease;
        }

        .result-message.win {
            background-color: #2ecc71;
            color: white;
        }

        .result-message.draw {
            background-color: #f39c12;
            color: white;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .game-actions {
            margin-top: 1rem;
            display: flex;
            justify-content: center;
            gap: 1rem;
        }

        .btn-small {
            padding: 0.5rem 1rem;
            font-size: 0.9rem;
            width: auto;
        }

        /* How to Play */
        .how-to-play-content {
            text-align: left;
        }

        .how-to-play-content h2 {
            text-align: center;
            margin-bottom: 1rem;
        }

        .example-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            grid-gap: 5px;
            margin: 1rem auto;
            max-width: 150px;
        }

        .example-cell {
            background-color: #f0f0f0;
            aspect-ratio: 1;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.5rem;
            font-weight: bold;
            border-radius: 3px;
        }

        .example-cell.x {
            color: #e74c3c;
        }

        .example-cell.o {
            color: #3498db;
        }

        .example-cell.highlight {
            background-color: rgba(46, 204, 113, 0.3);
        }

        /* Settings */
        .settings-content {
            text-align: left;
        }

        .settings-content h2 {
            text-align: center;
            margin-bottom: 1rem;
        }

        .setting-item {
            margin: 1.5rem 0;
        }

        .setting-item label {
            display: block;
            margin-bottom: 0.5rem;
            font-weight: bold;
        }

        .toggle-switch {
            position: relative;
            display: inline-block;
            width: 60px;
            height: 34px;
        }

        .toggle-switch input {
            opacity: 0;
            width: 0;
            height: 0;
        }

        .toggle-slider {
            position: absolute;
            cursor: pointer;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: #ccc;
            transition: .4s;
            border-radius: 34px;
        }

        .toggle-slider:before {
            position: absolute;
            content: "";
            height: 26px;
            width: 26px;
            left: 4px;
            bottom: 4px;
            background-color: white;
            transition: .4s;
            border-radius: 50%;
        }

        input:checked + .toggle-slider {
            background-color: #3498db;
        }

        input:checked + .toggle-slider:before {
            transform: translateX(26px);
        }

        .radio-group {
            display: flex;
            gap: 1rem;
        }

        .radio-option {
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }

        /* Icon styles */
        .icon {
            display: inline-block;
            width: 24px;
            height: 24px;
            margin-right: 10px;
            background-size: contain;
            background-repeat: no-repeat;
            background-position: center;
        }

        .icon-players {
            background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='white'%3E%3Cpath d='M16 7a4 4 0 11-8 0 4 4 0 018 0zm-4 7c-4.42 0-8 1.79-8 4v2h16v-2c0-2.21-3.58-4-8-4z'/%3E%3C/svg%3E");
        }

        .icon-robot {
            background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='white'%3E%3Cpath d='M20 9V7c0-1.1-.9-2-2-2h-3c0-1.66-1.34-3-3-3S9 3.34 9 5H6c-1.1 0-2 .9-2 2v2c-1.66 0-3 1.34-3 3s1.34 3 3 3v4c0 1.1.9 2 2 2h12c1.1 0 2-.9 2-2v-4c1.66 0 3-1.34 3-3s-1.34-3-3-3zm-2 8H6v-4h12v4zm0-8H6V7h12v2z'/%3E%3Ccircle cx='8.5' cy='11.5' r='1.5'/%3E%3Ccircle cx='15.5' cy='11.5' r='1.5'/%3E%3C/svg%3E");
        }

        .icon-help {
            background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='white'%3E%3Cpath d='M11 18h2v-2h-2v2zm1-16C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm0 18c-4.41 0-8-3.59-8-8s3.59-8 8-8 8 3.59 8 8-3.59 8-8 8zm0-14c-2.21 0-4 1.79-4 4h2c0-1.1.9-2 2-2s2 .9 2 2c0 2-3 1.75-3 5h2c0-2.25 3-2.5 3-5 0-2.21-1.79-4-4-4z'/%3E%3C/svg%3E");
        }

        .icon-settings {
            background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='white'%3E%3Cpath d='M19.14 12.94c.04-.3.06-.61.06-.94 0-.32-.02-.64-.07-.94l2.03-1.58c.18-.14.23-.41.12-.61l-1.92-3.32c-.12-.22-.37-.29-.59-.22l-2.39.96c-.5-.38-1.03-.7-1.62-.94l-.36-2.54c-.04-.24-.24-.41-.48-.41h-3.84c-.24 0-.43.17-.47.41l-.36 2.54c-.59.24-1.13.56-1.62.94l-2.39-.96c-.22-.08-.47 0-.59.22L2.74 8.87c-.12.21-.08.47.12.61l2.03 1.58c-.05.3-.09.63-.09.94s.02.64.07.94l-2.03 1.58c-.18.14-.23.41-.12.61l1.92 3.32c.12.22.37.29.59.22l2.39-.96c.5.38 1.03.7 1.62.94l.36 2.54c.05.24.24.41.48.41h3.84c.24 0 .44-.17.47-.41l.36-2.54c.59-.24 1.13-.56 1.62-.94l2.39.96c.22.08.47 0 .59-.22l1.92-3.32c.12-.22.07-.47-.12-.61l-2.01-1.58zM12 15.6c-1.98 0-3.6-1.62-3.6-3.6s1.62-3.6 3.6-3.6 3.6 1.62 3.6 3.6-1.62 3.6-3.6 3.6z'/%3E%3C/svg%3E");
        }

        /* Stats */
        .stats {
            display: flex;
            justify-content: space-around;
            margin: 1rem 0;
        }

        .stat-item {
            text-align: center;
            padding: 0.5rem;
            background-color: #f0f0f0;
            border-radius: 5px;
            min-width: 70px;
        }

        .stat-value {
            font-size: 1.5rem;
            font-weight: bold;
        }

        .stat-label {
            font-size: 0.8rem;
            color: #555;
        }

        /* Sound effect element */
        .sound-effect-player {
            display: none;
        }

        /* Responsive */
        @media (max-width: 600px) {
            .main-screen, .game-screen, .how-to-play-screen, .settings-screen {
                padding: 1rem;
            }

            header h1 {
                font-size: 1.5rem;
            }

            .game-board {
                max-width: 250px;
            }

            .cell {
                font-size: 2rem;
            }
        }
    </style>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
</head>
<body>
    <div class="bg-pattern"></div>
    <header>
        <h1>Tic Tac Toe</h1>
    </header>

    <!-- Main Menu Screen -->
    <div id="main-screen" class="screen active">
        <div class="welcome-message">
            <p>Welcome to Tic Tac Toe! Choose a game mode to start playing.</p>
        </div>
        <button id="two-player-btn" class="btn">
            <span class="icon icon-players"></span>
            TWO PLAYER MODE
        </button>
        <button id="single-player-btn" class="btn">
            <span class="icon icon-robot"></span>
            SINGLE PLAYER MODE
        </button>
        <button id="how-to-play-btn" class="btn">
            <span class="icon icon-help"></span>
            HOW TO PLAY
        </button>
        <button id="settings-btn" class="btn">
            <span class="icon icon-settings"></span>
            SETTINGS
        </button>
    </div>

    <!-- Game Screen -->
    <div id="game-screen" class="screen">
        <div class="game-info">
            <div id="player-turn" class="player-turn x">Player X's Turn</div>
        </div>
        <div class="game-board" id="game-board">
            <div class="cell" data-index="0"></div>
            <div class="cell" data-index="1"></div>
            <div class="cell" data-index="2"></div>
            <div class="cell" data-index="3"></div>
            <div class="cell" data-index="4"></div>
            <div class="cell" data-index="5"></div>
            <div class="cell" data-index="6"></div>
            <div class="cell" data-index="7"></div>
            <div class="cell" data-index="8"></div>
        </div>
        <div id="result-message" class="result-message"></div>

        <div class="stats">
            <div class="stat-item">
                <div id="x-wins" class="stat-value">0</div>
                <div class="stat-label">X Wins</div>
            </div>
            <div class="stat-item">
                <div id="o-wins" class="stat-value">0</div>
                <div class="stat-label">O Wins</div>
            </div>
            <div class="stat-item">
                <div id="draws" class="stat-value">0</div>
                <div class="stat-label">Draws</div>
            </div>
        </div>

        <div class="game-actions">
            <button id="play-again-btn" class="btn btn-small">Play Again</button>
            <button id="back-to-menu-btn" class="btn btn-small">Main Menu</button>
        </div>
    </div>

    <!-- How To Play Screen -->
    <div id="how-to-play-screen" class="screen">
        <div class="how-to-play-content">
            <h2>How To Play</h2>
            <p>Tic Tac Toe is played on a 3x3 grid. Players take turns marking a square with their symbol (X or O).</p>
            <p>The first player to get 3 of their symbols in a row (horizontally, vertically, or diagonally) wins the game.</p>
            <p>If all squares are filled and no player has 3 in a row, the game ends in a draw.</p>
            
            <h3>Example Win Condition:</h3>
            <div class="example-grid">
                <div class="example-cell x highlight">X</div>
                <div class="example-cell"></div>
                <div class="example-cell o">O</div>
                <div class="example-cell x highlight">X</div>
                <div class="example-cell o">O</div>
                <div class="example-cell"></div>
                <div class="example-cell x highlight">X</div>
                <div class="example-cell"></div>
                <div class="example-cell"></div>
            </div>
            <p>In this example, X wins with a diagonal line.</p>
        </div>
        <button id="back-from-how-to-play" class="btn">Back to Menu</button>
    </div>

    <!-- Settings Screen -->
    <div id="settings-screen" class="screen">
        <div class="settings-content">
            <h2>Settings</h2>
            
            <div class="setting-item">
                <label>Sound Effects</label>
                <label class="toggle-switch">
                    <input type="checkbox" id="sound-toggle" checked>
                    <span class="toggle-slider"></span>
                </label>
            </div>
            
            <div class="setting-item">
                <label>Theme</label>
                <label class="toggle-switch">
                    <input type="checkbox" id="theme-toggle">
                    <span class="toggle-slider"></span>
                </label>
                <span id="theme-label">Light</span>
            </div>
            
            <div class="setting-item">
                <label>AI Difficulty (Single Player)</label>
                <div class="radio-group">
                    <div class="radio-option">
                        <input type="radio" id="easy-mode" name="difficulty" value="easy" checked>
                        <label for="easy-mode">Easy</label>
                    </div>
                    <div class="radio-option">
                        <input type="radio" id="hard-mode" name="difficulty" value="hard">
                        <label for="hard-mode">Hard</label>
                    </div>
                </div>
            </div>
            
            <div class="setting-item">
                <label>Player Symbol (Single Player)</label>
                <div class="radio-group">
                    <div class="radio-option">
                        <input type="radio" id="player-x" name="player-symbol" value="X" checked>
                        <label for="player-x">X (First)</label>
                    </div>
                    <div class="radio-option">
                        <input type="radio" id="player-o" name="player-symbol" value="O">
                        <label for="player-o">O (Second)</label>
                    </div>
                </div>
            </div>
        </div>
        <button id="back-from-settings" class="btn">Save & Back</button>
    </div>

    <!-- Sound Effects -->
    <audio id="move-sound" class="sound-effect-player">
        <source src="data:audio/mpeg;base64,SUQzBAAAAAAAI1RTU0UAAAAPAAADTGF2ZjU4Ljc2LjEwMAAAAAAAAAAAAAAA/+M4wAAAAAAAAAAAAEluZm8AAAAPAAAAAwAAABIAEhIiIiIiMTExMTExQUFBQUFBUVFRUVFRYWFhYWFhcXFxcXFxgYGBgYGBkZGRkZGRoaGhoaGhsbGxsbGxwcHBwcHB0dHR0dHR4eHh4eHh8fHx8fHx//////////////////////////8AAAAATGF2YzU4LjEzAAAAAAAAAAAAAAAAJAJYgQAB//MUxAADwAIBGiAAAIAgIBCCQdd3d3iBOHiBOHiBOECAh3hAQ7u7vCBBAQcd3eECXeIAgEOEBA7u7uEBAIdd3d3d3d4QEO7u7w7vCBLvd3iA7u8O8O7wiHd4d3h3d3eECXd3iHeEQ7vEO8I" type="audio/mpeg">
    </audio>
    <audio id="win-sound" class="sound-effect-player">
        <source src="data:audio/mpeg;base64,SUQzBAAAAAAAI1RTU0UAAAAPAAADTGF2ZjU4Ljc2LjEwMAAAAAAAAAAAAAAA/+M4wAAAAAAAAAAAAEluZm8AAAAPAAAAAwAAABIAEhIiIiIiMTExMTExQUFBQUFBUVFRUVFRYWFhYWFhcXFxcXFxgYGBgYGBkZGRkZGRoaGhoaGhsbGxsbGxwcHBwcHB0dHR0dHR4eHh4eHh8fHx8fHx//////////////////////////8AAAAATGF2YzU4LjEzAAAAAAAAAAAAAAAAJAZGgQAB//MUxAADgAIDGiAAAIAgIBDd//++QcB//5BwjUCAhmQcJ1AgUdkHYOEbATKOwhUCAhmQcI1AgIJkHCNQICCZBwjUCAhmQcI1AgIJkHCNQICCZBwjUCAhmQcI1AgIZkHCdQICPu7vCHd4QEE//MUxBcXM78eJlmMAESZBwjUCAgmQcI1AgIJkHCNQICCZBwjUCAhmQcJ1AgIMg7CdQICD/++QcB//5BwjUCAgmQcI1AgIZkHCNQICCZBwjUCAgmQcI1AgIZkHCNQICCZBwjUCAgmQcJ1A//MUxA8WA8ceABmMAGZBwgMH/d3CEByDgQAH/d3hAgQHeBAnDvDvGiI9VdEQ6qroiHRUMRDfD4iG+HxEN8PoGPCDBjwgwY8EDB4YE7JZrJZrJZrYLBDUEg1BIVCGHo9Ho9Ojw+Hh//MUxA8VU5MaQDGGAN3d3d3d3d3d3d3d3d3d3giOUB3giI5QHeCId4IiIiIiIiIiIiIiIiIhETM7u7M3d3d3d3d3d3d3d3d3d3d3d3d3d3d3d3d3d3d3d3d3d3d3d3d3d3d3d3d3d3d3" type="audio/mpeg">
    </audio>
    <audio id="draw-sound" class="sound-effect-player">
        <source src="data:audio/mpeg;base64,SUQzBAAAAAAAI1RTU0UAAAAPAAADTGF2ZjU4Ljc2LjEwMAAAAAAAAAAAAAAA/+M4wAAAAAAAAAAAAEluZm8AAAAPAAAAAwAAABIAEhIiIiIiMTExMTExQUFBQUFBUVFRUVFRYWFhYWFhcXFxcXFxgYGBgYGBkZGRkZGRoaGhoaGhsbGxsbGxwcHBwcHB0dHR0dHR4eHh4eHh8fHx8fHx//////////////////////////8AAAAATGF2YzU4LjEzAAAAAAAAAAAAAAAAJAXJgQAB//MUxAADwAIBGiAAAP8QEBrd3eICAQId3eHd4gIBAnd3d3eICAQJ3d4d3iBAgCECBAgCECBAgCECAgECAgECBAgCCBAgQIECBAjU1NTUSrb60qS3xqamYIECBAjU1NTUSrb60qS0//MUxBUUa98WIBGKAdTU1NTUggQIECNTU1Eqtq0qSE443Nt7e4uMXGLnHHHGwwwxcdHRwuONhcXONzbe3xdsTERESIiIVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVV//MUxBcXs5saQBmSAFVVVVVVEiIiIiRVVVVVVVVVVVVVVVVVVVVVVVVVVVEREREUiIiIiFVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVV" type="audio/mpeg">
    </audio>

    <script>
        // Game state
        const gameState = {
            board: Array(9).fill(null),
            currentPlayer: 'X',
            gameActive: true,
            gameMode: 'two-player', // 'two-player' or 'single-player'
            aiDifficulty: 'easy', // 'easy' or 'hard'
            playerSymbol: 'X', // For single-player mode
            soundEnabled: true,
            darkTheme: false,
            stats: {
                xWins: 0,
                oWins: 0,
                draws: 0
            }
        };

        // DOM elements
        const screens = {
            main: document.getElementById('main-screen'),
            game: document.getElementById('game-screen'),
            howToPlay: document.getElementById('how-to-play-screen'),
            settings: document.getElementById('settings-screen')
        };

        const gameElements = {
            board: document.getElementById('game-board'),
            cells: document.querySelectorAll
