<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Roblox Web Launcher</title>
    <style>
        /* This removes all default spacing from the browser */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body, html {
            width: 100%;
            height: 100%;
            overflow: hidden; /* This prevents scrolling */
            background-color: #000;
        }

        /* This container forces the game to be exactly the size of the screen */
        .game-wrapper {
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            display: flex;
        }

        iframe {
            width: 100%;
            height: 100%;
            border: none;
            display: block;
        }

        /* Optional: A tiny hidden refresh button in the corner */
        .refresh-zone {
            position: fixed;
            bottom: 0;
            right: 0;
            width: 40px;
            height: 40px;
            z-index: 999;
            cursor: pointer;
            opacity: 0; /* Change to 0.1 to see it slightly */
        }
    </style>
</head>
<body>

    <div class="refresh-zone" onclick="location.reload()" title="Refresh Game"></div>

    <div class="game-wrapper">
        <iframe 
            src="https://www.easyfun.gg/games/roblox.html" 
            allow="autoplay; fullscreen; keyboard"
            sandbox="allow-forms allow-scripts allow-same-origin allow-popups allow-popups-to-escape-sandbox">
        </iframe>
    </div>

</body>
</html>
