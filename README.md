<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SoleminerCloud</title>
    <link rel="icon" type="image/png" href="https://img.icons8.com/neon/96/000000/controller.png">
    <style>
        :root {
            --neon-green: #00ff88;
            --dark-bg: #0d0d0f;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; cursor: crosshair; }
        body, html { width: 100%; height: 100%; overflow: hidden; background-color: #000; font-family: 'Consolas', monospace; }

        /* The Game Frame */
        .app-container { position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; z-index: 1; }
        iframe { width: 100%; height: 100%; border: none; }

        /* FEATURE 1: HUD Stats (Bottom Left) */
        .hud-stats {
            position: fixed;
            bottom: 15px;
            left: 20px;
            z-index: 10;
            color: var(--neon-green);
            font-size: 12px;
            text-shadow: 0 0 5px var(--neon-green);
            pointer-events: none;
            background: rgba(0,0,0,0.5);
            padding: 10px;
            border-left: 2px solid var(--neon-green);
        }

        /* FEATURE 2: Ownership Tag (Bottom Right) */
        .ownership-tag {
            position: fixed;
            bottom: 15px;
            right: 20px;
            background: rgba(13, 13, 15, 0.85);
            color: var(--neon-green);
            padding: 10px 15px;
            border-radius: 4px;
            font-size: 12px;
            font-weight: bold;
            border: 1px solid rgba(0, 255, 136, 0.3);
            z-index: 10;
            pointer-events: none;
            text-transform: uppercase;
        }

        /* Loading Screen */
        #loading-overlay {
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: var(--dark-bg);
            display: flex; flex-direction: column; justify-content: center; align-items: center;
            z-index: 9999; transition: opacity 1s ease;
        }
        .loader-ring {
            width: 70px; height: 70px;
            border: 5px solid #1a1a1a; border-top: 5px solid var(--neon-green);
            border-radius: 50%; animation: spin 0.8s linear infinite;
        }
        .brand-name {
            margin-top: 25px; color: var(--neon-green); font-size: 28px;
            font-weight: 900; letter-spacing: 4px; text-transform: uppercase;
        }
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
    </style>
</head>
<body>

    <div id="loading-overlay">
        <div class="loader-ring"></div>
        <div class="brand-name">SoleminerCloud</div>
        <p style="color: #555; font-size: 11px; margin-top: 10px;">OPTIMIZING STREAM...</p>
    </div>

    <div class="hud-stats">
        <div id="clock">00:00:00</div>
        <div id="battery">BATTERY: --%</div>
        <div style="font-size: 9px; margin-top: 5px; opacity: 0.7;">CONNECTION: STABLE</div>
    </div>

    <div class="ownership-tag">
        Owned by SoleminerCloud
    </div>

    <div class="app-container">
        <iframe 
            src="https://web.cloudmoonapp.com/" 
            allow="autoplay; fullscreen; keyboard"
            sandbox="allow-forms allow-scripts allow-same-origin allow-popups allow-popups-to-escape-sandbox">
        </iframe>
    </div>

    <script>
        // 1. DIGITAL CLOCK LOGIC
        function updateClock() {
            const now = new Date();
            document.getElementById('clock').innerText = now.toLocaleTimeString();
        }
        setInterval(updateClock, 1000);
        updateClock();

        // 2. BATTERY MONITOR LOGIC
        if (navigator.getBattery) {
            navigator.getBattery().then(function(battery) {
                function updateBattery() {
                    document.getElementById('battery').innerText = "BATTERY: " + Math.round(battery.level * 100) + "%";
                }
                updateBattery();
                battery.addEventListener('levelchange', updateBattery);
            });
        }

        // 3. PANIC BUTTON: Key [7] -> Satchel One
        document.addEventListener('keydown', function(event) {
            if (event.key === "7") {
                window.location.replace("https://www.satchelone.com/login");
            }
        });

        // FOCUS KEEPER
        setInterval(function() { window.focus(); }, 1000);

        // HIDE OVERLAY
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
