<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SoleminerCloud</title>
    <link rel="icon" type="image/png" href="https://img.icons8.com/neon/96/000000/controller.png">
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body, html { width: 100%; height: 100%; overflow: hidden; background-color: #000; font-family: 'Segoe UI', sans-serif; }

        .app-container {
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
        }

        iframe { width: 100%; height: 100%; border: none; }

        /* THE MASK: This covers the top area where Cloud Moon usually shows its logo */
        .soleminer-mask {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 45px; /* Adjust height if needed to cover their logo */
            background: #0d0d0f;
            z-index: 10;
            display: flex;
            align-items: center;
            padding: 0 20px;
            border-bottom: 1px solid #00ff88;
        }

        .mask-text {
            color: #00ff88;
            font-weight: 900;
            letter-spacing: 2px;
            font-size: 14px;
            text-transform: uppercase;
        }

        /* CUSTOM LOADING SCREEN */
        #loading-overlay {
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: #0d0d0f;
            display: flex; flex-direction: column; justify-content: center; align-items: center;
            z-index: 9999; transition: opacity 1.2s ease;
        }

        .loader-ring {
            width: 80px; height: 80px;
            border: 6px solid #1a1a1a; border-top: 6px solid #00ff88;
            border-radius: 50%; animation: spin 0.8s linear infinite;
        }

        .brand-name {
            margin-top: 30px; color: #00ff88; font-size: 32px;
            font-weight: 900; letter-spacing: 6px; text-transform: uppercase;
        }

        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
    </style>
</head>
<body>

    <div id="loading-overlay">
        <div class="loader-ring"></div>
        <div class="brand-name">SoleminerCloud</div>
        <p style="color: #555; font-size: 12px; margin-top: 10px;">PREPARING PRIVATE INSTANCE...</p>
    </div>

    <div class="soleminer-mask">
        <div class="mask-text">SoleminerCloud System</div>
    </div>

    <div class="app-container">
        <iframe 
            src="https://web.cloudmoonapp.com/" 
            allow="autoplay; fullscreen; keyboard"
            sandbox="allow-forms allow-scripts allow-same-origin allow-popups allow-popups-to-escape-sandbox">
        </iframe>
    </div>

    <script>
        // PANIC BUTTON: Key [7] -> Satchel One
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
                setTimeout(() => { loader.style.display = 'none'; }, 1200);
            }, 6000); // 6 seconds to ensure their logo is gone
        });
    </script>
</body>
</html>
