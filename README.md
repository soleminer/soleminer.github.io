<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Soleminer Labs | Games Collection</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;700;900&family=JetBrains+Mono&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #050505;
            color: white;
            overflow-x: hidden;
        }
        .mono { font-family: 'JetBrains Mono', monospace; }
        .glass {
            background: rgba(23, 23, 23, 0.6);
            backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        canvas { display: block; }
        #game-container { aspect-ratio: 16/9; }
        .crosshair {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            pointer-events: none;
            z-index: 50;
        }
    </style>
</head>
<body class="min-h-screen flex flex-col items-center">

    <!-- Grid Background -->
    <div class="fixed inset-0 pointer-events-none opacity-20">
        <div class="absolute inset-0" style="background-image: linear-gradient(to right, #80808012 1px, transparent 1px), linear-gradient(to bottom, #80808012 1px, transparent 1px); background-size: 40px 40px;"></div>
    </div>

    <!-- Header -->
    <header class="w-full max-w-7xl py-8 px-8 flex flex-col md:flex-row items-center justify-between gap-8 z-10">
        <div class="flex items-center gap-5 cursor-pointer" onclick="showView('menu')">
            <div class="w-14 h-14 bg-white rounded-2xl flex items-center justify-center text-black shadow-xl rotate-2">
                <svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><rect width="18" height="11" x="3" y="11" rx="2" ry="2"/><path d="M7 11V7a5 5 0 0 1 10 0v4"/><circle cx="8" cy="15" r="1"/><circle cx="16" cy="15" r="1"/></svg>
            </div>
            <div>
                <h1 class="text-3xl font-black tracking-tighter uppercase italic">Soleminer<span class="text-emerald-500">Labs</span></h1>
                <div class="flex items-center gap-2 mt-1">
                    <span class="text-[10px] font-mono text-white/40 tracking-[0.4em] uppercase">Voxel Engine v3.0</span>
                    <span class="w-1.5 h-1.5 rounded-full bg-emerald-500 animate-pulse"></span>
                </div>
            </div>
        </div>
        <div id="nav-back" class="hidden">
            <button onclick="showView('menu')" class="bg-white/5 hover:bg-white/10 px-6 py-2 rounded-xl border border-white/10 text-xs font-black uppercase tracking-widest transition-all">
                Back to Menu
            </button>
        </div>
    </header>

    <!-- Main Content -->
    <main class="w-full max-w-7xl px-8 pb-20 z-10">
        
        <!-- Menu View -->
        <div id="view-menu" class="w-full">
            <h2 class="text-neutral-500 text-xs font-black uppercase tracking-[0.4em] mb-12 text-center">Select_Interactive_Environment</h2>
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
                <!-- Voxel Card -->
                <div onclick="showView('voxel')" class="bg-neutral-900/40 p-8 rounded-[2rem] border border-white/5 hover:border-emerald-500/30 hover:bg-neutral-900/60 transition-all duration-500 cursor-pointer group">
                    <div class="bg-emerald-500 w-16 h-16 rounded-2xl flex items-center justify-center text-black mb-6 group-hover:scale-110 transition-transform shadow-lg">
                        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m21 8-9-4-9 4V16l9 4 9-4V8Z"/><path d="m3 8 9 4 9-4"/><path d="M12 12v8"/></svg>
                    </div>
                    <h3 class="text-2xl font-black tracking-tight mb-3 uppercase italic">Voxel World</h3>
                    <p class="text-neutral-500 text-sm leading-relaxed mb-6">Custom 3D sandbox with trees and terrain.</p>
                </div>
                <!-- Neon Card -->
                <div onclick="showView('neon')" class="bg-neutral-900/40 p-8 rounded-[2rem] border border-white/5 hover:border-amber-500/30 hover:bg-neutral-900/60 transition-all duration-500 cursor-pointer group">
                    <div class="bg-amber-500 w-16 h-16 rounded-2xl flex items-center justify-center text-black mb-6 group-hover:scale-110 transition-transform shadow-lg">
                        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M13 2L3 14h9l-1 8 10-12h-9l1-8z"/></svg>
                    </div>
                    <h3 class="text-2xl font-black tracking-tight mb-3 uppercase italic">Neon Dash</h3>
                    <p class="text-neutral-500 text-sm leading-relaxed mb-6">High-speed geometric platforming.</p>
                </div>
                <!-- Roblox Card -->
                <div onclick="showView('roblox')" class="bg-neutral-900/40 p-8 rounded-[2rem] border border-white/5 hover:border-red-500/30 hover:bg-neutral-900/60 transition-all duration-500 cursor-pointer group">
                    <div class="bg-red-500 w-16 h-16 rounded-2xl flex items-center justify-center text-black mb-6 group-hover:scale-110 transition-transform shadow-lg">
                        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect width="16" height="12" x="2" y="8" rx="2"/><path d="M7 8V5a5 5 0 0 1 10 0v3"/></svg>
                    </div>
                    <h3 class="text-2xl font-black tracking-tight mb-3 uppercase italic">Roblox</h3>
                    <p class="text-neutral-500 text-sm leading-relaxed mb-6">Official access to millions of 3D games.</p>
                </div>
                <!-- Bloxd Card -->
                <div onclick="showView('bloxd')" class="bg-neutral-900/40 p-8 rounded-[2rem] border border-white/5 hover:border-blue-500/30 hover:bg-neutral-900/60 transition-all duration-500 cursor-pointer group">
                    <div class="bg-blue-500 w-16 h-16 rounded-2xl flex items-center justify-center text-black mb-6 group-hover:scale-110 transition-transform shadow-lg">
                        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M14.5 7L22 22"/><path d="M10 2l6 6"/><path d="m2 22 7.5-7.5"/><path d="m5 15 7-7"/></svg>
                    </div>
                    <h3 class="text-2xl font-black tracking-tight mb-3 uppercase italic">Bloxd.io</h3>
                    <p class="text-neutral-500 text-sm leading-relaxed mb-6">Online multiplayer voxel building.</p>
                </div>
            </div>
        </div>

        <!-- Game Container -->
        <div id="game-view" class="hidden w-full">
            <div class="relative w-full max-w-5xl mx-auto rounded-[3rem] p-2 bg-gradient-to-br from-white/10 to-transparent border border-white/10 shadow-2xl overflow-hidden bg-black">
                <div id="game-mount" class="w-full h-full rounded-[2.6rem] overflow-hidden relative min-h-[500px]">
                    <!-- Voxel Overlay -->
                    <div id="voxel-lock-screen" class="hidden absolute inset-0 bg-black/80 backdrop-blur-md flex flex-col items-center justify-center text-center p-6 z-20">
                        <div class="bg-emerald-500/20 p-6 rounded-full mb-6 border border-emerald-500/30">
                            <svg xmlns="http://www.w3.org/2000/svg" width="48" height="48" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="text-emerald-400"><polygon points="5 3 19 12 5 21 5 3"/></svg>
                        </div>
                        <h3 class="text-4xl font-black mb-3 tracking-tighter uppercase italic">Enter World</h3>
                        <p class="text-neutral-400 mb-8">Click to lock mouse and play. WASD to move.</p>
                    </div>
                    <!-- Voxel HUD -->
                    <div id="voxel-hud" class="hidden absolute bottom-8 left-1/2 -translate-x-1/2 flex gap-3 bg-black/60 p-4 rounded-3xl backdrop-blur-2xl border border-white/10 z-20">
                        <div class="block-btn w-12 h-12 rounded-xl border-2 border-emerald-500" style="background: #54ba45"></div>
                        <div class="block-btn w-12 h-12 rounded-xl" style="background: #78350f"></div>
                        <div class="block-btn w-12 h-12 rounded-xl" style="background: #64748b"></div>
                        <div class="block-btn w-12 h-12 rounded-xl" style="background: #5d4037"></div>
                        <div class="block-btn w-12 h-12 rounded-xl" style="background: #2e7d32"></div>
                    </div>
                    <!-- Iframe Container -->
                    <iframe id="game-frame" class="hidden w-full h-full min-h-[500px] border-none"></iframe>
                    <!-- Canvas Container -->
                    <div id="voxel-canvas-container" class="hidden w-full h-full min-h-[500px]"></div>
                </div>
            </div>
        </div>
    </main>

    <script>
        const views = {
            menu: document.getElementById('view-menu'),
            game: document.getElementById('game-view'),
            back: document.getElementById('nav-back'),
            frame: document.getElementById('game-frame'),
            voxel: document.getElementById('voxel-canvas-container'),
            voxelLock: document.getElementById('voxel-lock-screen'),
            voxelHud: document.getElementById('voxel-hud')
        };

        let voxelEngine = null;

        function showView(viewId) {
            // Reset
            Object.values(views).forEach(v => v.classList.add('hidden'));
            views.frame.src = "";
            if (voxelEngine) voxelEngine.stop();

            if (viewId === 'menu') {
                views.menu.classList.remove('hidden');
            } else {
                views.game.classList.remove('hidden');
                views.back.classList.remove('hidden');

                if (viewId === 'voxel') {
                    views.voxel.classList.remove('hidden');
                    views.voxelLock.classList.remove('hidden');
                    views.voxelHud.classList.remove('hidden');
                    startVoxel();
                } else if (viewId === 'neon') {
                    views.frame.classList.remove('hidden');
                    // In a real HTML file, you'd put the Neon game logic in a separate file or inline it here
                    // For this example, we'll focus on the primary 3D engine
                    views.frame.srcdoc = "<html><body style='background:black;color:white;display:flex;align-items:center;justify-content:center;font-family:sans-serif'><h1>Neon Dash is loading...</h1></body></html>";
                } else if (viewId === 'roblox') {
                    views.frame.classList.remove('hidden');
                    views.frame.src = "https://www.roblox.com";
                } else if (viewId === 'bloxd') {
                    views.frame.classList.remove('hidden');
                    views.frame.src = "https://bloxd.io";
                }
            }
        }

        function startVoxel() {
            const container = views.voxel;
            container.innerHTML = "";
            
            const scene = new THREE.Scene();
            scene.background = new THREE.Color(0x0a0a0a);
            scene.fog = new THREE.FogExp2(0x0a0a0a, 0.02);

            const camera = new THREE.PerspectiveCamera(75, container.offsetWidth / 500, 0.1, 1000);
            const renderer = new THREE.WebGLRenderer({ antialias: true });
            renderer.setSize(container.offsetWidth, 500);
            container.appendChild(renderer.domElement);

            const light = new THREE.DirectionalLight(0xffffff, 1);
            light.position.set(10, 20, 10);
            scene.add(light);
            scene.add(new THREE.AmbientLight(0x404040, 2.0));

            const geometry = new THREE.BoxGeometry(1, 1, 1);
            const materials = [
                new THREE.MeshLambertMaterial({ color: 0x54ba45 }),
                new THREE.MeshLambertMaterial({ color: 0x78350f }),
                new THREE.MeshLambertMaterial({ color: 0x64748b }),
                new THREE.MeshLambertMaterial({ color: 0x5d4037 }),
                new THREE.MeshLambertMaterial({ color: 0x2e7d32 }),
            ];

            const world = new THREE.Group();
            scene.add(world);

            const createBlock = (x, y, z, typeIdx) => {
                const mesh = new THREE.Mesh(geometry, materials[typeIdx]);
                mesh.position.set(x, y, z);
                world.add(mesh);
            };

            // Terrain
            for (let x = -15; x < 15; x++) {
                for (let z = -15; z < 15; z++) {
                    const h = Math.floor(Math.sin(x * 0.15) * 2 + Math.cos(z * 0.15) * 2);
                    for (let y = -3; y <= h; y++) {
                        let type = 2; // Stone/Dirt
                        if (y === h) type = 0; // Grass
                        createBlock(x, y, z, type);
                    }
                    // Random Trees
                    if (x % 8 === 0 && z % 8 === 0 && Math.random() > 0.5) {
                        for(let th=1; th<5; th++) createBlock(x, h+th, z, 3); // Trunk
                        for(let lx=-1; lx<=1; lx++) 
                            for(let lz=-1; lz<=1; lz++) 
                                createBlock(x+lx, h+5, z+lz, 4); // Leaves
                    }
                }
            }

            camera.position.set(0, 5, 10);
            let selectedType = 0;
            const keys = {};

            document.addEventListener('keydown', (e) => keys[e.code] = true);
            document.addEventListener('keyup', (e) => keys[e.code] = false);
            
            container.addEventListener('mousedown', () => {
                renderer.domElement.requestPointerLock();
            });

            document.addEventListener('pointerlockchange', () => {
                if (document.pointerLockElement === renderer.domElement) {
                    views.voxelLock.classList.add('hidden');
                } else {
                    views.voxelLock.classList.remove('hidden');
                }
            });

            const euler = new THREE.Euler(0, 0, 0, 'YXZ');
            document.addEventListener('mousemove', (e) => {
                if (document.pointerLockElement !== renderer.domElement) return;
                euler.y -= e.movementX * 0.002;
                euler.x -= e.movementY * 0.002;
                euler.x = Math.max(-Math.PI/2, Math.PI/2, euler.x);
                camera.quaternion.setFromEuler(euler);
            });

            function animate() {
                if (!container.innerHTML) return;
                requestAnimationFrame(animate);
                
                const speed = 0.1;
                const dir = new THREE.Vector3();
                camera.getWorldDirection(dir);
                dir.y = 0; dir.normalize();
                const side = new THREE.Vector3().crossVectors(camera.up, dir).normalize();

                if (keys['KeyW']) camera.position.addScaledVector(dir, speed);
                if (keys['KeyS']) camera.position.addScaledVector(dir, -speed);
                if (keys['KeyA']) camera.position.addScaledVector(side, speed);
                if (keys['KeyD']) camera.position.addScaledVector(side, -speed);
                if (keys['Space']) camera.position.y += 0.1;
                if (keys['ShiftLeft']) camera.position.y -= 0.1;

                renderer.render(scene, camera);
            }
            animate();
            
            voxelEngine = { stop: () => { container.innerHTML = ""; } };
        }
    </script>
</body>
</html>
