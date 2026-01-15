<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Roblox Fullscreen Portal</title>
    <style>
        /* This makes the page take up 100% of the browser window */
        body, html {
            margin: 0;
            padding: 0;
            height: 100%;
            width: 100%;
            overflow: hidden; /* Prevents scrollbars */
            background-color: #000;
        }

        .game-container {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
        }

        iframe {
            width: 100%;
            height: 100%;
            border: none;
        }

        /* Small button to go back or refresh if stuck */
        #menu {
            position: fixed;
            bottom: 10px;
            right: 10px;
            background: rgba(0,0,0,0.5);
            padding: 5px 10px;
            border-radius: 5px;
            color: white;
            font-family: sans-serif;
            font-size: 10px;
            z-index: 100;
            cursor: pointer;
            opacity: 0.3;
        }
        #menu:hover { opacity: 1; }
    </style>
</head>
<body>

    <div id="menu" onclick="location.reload()">REFRESH GAME</div>

    <div class="game-container">
        <iframe 
            src="https://www.easyfun.gg/games/roblox.html" 
            allow="autoplay; fullscreen; keyboard"
            sandbox="allow-forms allow-scripts allow-same-origin allow-popups">
        </iframe>
    </div>

</body>
</html>
