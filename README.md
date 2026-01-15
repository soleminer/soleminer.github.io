<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SoleminerCloud</title>

    <link rel="icon" type="image/png" href="https://img.icons8.com/neon/96/000000/controller.png">

    <style>
        /* Complete UI Reset */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body, html {
            width: 100%;
            height: 100%;
            overflow: hidden;
            background-color: #000;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        /* Full Screen Game Container */
        .app-container {
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            background: #000;
        }

        iframe {
            width: 100%;
            height: 100%;
            border: none;
            display: block;
        }

        /* SoleminerCloud Loading Screen */
        #loading-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: #0d0d0f;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 9999;
            transition: opacity 1s ease;
        }

        .loader-ring {
            width: 70px;
            height: 70px;
            border: 5px solid #1a1a1a;
            border-top: 5px solid #00ff88;
            border-radius: 50%;
            animation: spin 0.8s linear infinite;
        }

        .brand-name {
            margin-top: 25px;
            color: #00ff88;
            font-size: 26px;
            font-weight: 900;
            letter-spacing: 5px;
            text-transform: uppercase;
            text-shadow: 0 0 15px rgba(0, 255, 136, 0.4);
        }

        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }

        /* Very faint tag to show the site is active */
        .system-dot {
            position: fixed;
            bottom: 10px;
            left: 10px;
            width: 4px;
            height: 4px;
            background: rgba(0, 255, 136, 0.1);
            border-radius: 50%;
            z-index: 100;
        }
    </style>
</head>
<body>

    <div id="loading-overlay">
        <div class="loader-ring"></div>
        <div class="brand-name">SoleminerCloud</div>
        <p style="color: #555; font-size: 11px; margin-top: 15px; letter-spacing: 1px;">ENCRYPTED CLOUD SESSION</p>
    </div>

    <div class="system-dot"></div>

    <div class="app-container">
        <iframe 
            id="game-frame"
            src="https://web.cloudmoonapp.com/" 
            allow="autoplay; fullscreen; keyboard"
            sandbox="allow-forms allow-scripts allow-same-origin allow-popups allow-popups-to-escape-sandbox">
        </iframe>
    </div>

    <script>
        /**
         * PANIC SYSTEM
         * Key: 7 
         * Target: Satchel One
         */
        function triggerPanic() {
            window.location.replace("https://www.satchelone.com/login");
        }

        document.addEventListener('keydown', function(event) {
            if (event.key === "7") {
                triggerPanic();
            }
        });

        /**
         * FOCUS KEEPER
         * Keeps the browser alert for the '7' key even during gameplay
         */
        setInterval(function() {
            window.focus();
        }, 1000);

        /**
         * LOADER CONTROL
         * Hides the custom SoleminerCloud loading screen once ready
         */
        window.addEventListener('load', function() {
            setTimeout(function() {
                const loader = document.getElementById('loading-overlay');
                loader.style.opacity = '0';
                setTimeout(() => { loader.style.display = 'none'; }, 1000);
            }, 4000);
        });
    </script>
</body>
</html>
