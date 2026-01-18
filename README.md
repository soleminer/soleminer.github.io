import React, { useState, useEffect, useRef } from 'react';
import { Pickaxe, Box, Zap, Gamepad2, MousePointer2, Move, ShieldCheck, TreeDeciduous, Info, Play, ChevronRight, ExternalLink, Monitor, Globe } from 'lucide-react';

// --- THREE.JS IMPORT ---
const THREE_JS_URL = "https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js";

// --- Minecraft 3D Component ---
const Minecraft3D = () => {
  const mountRef = useRef(null);
  const rendererRef = useRef(null);
  const [selected, setSelected] = useState(1);
  const [isLocked, setIsLocked] = useState(false);
  const [loaded, setLoaded] = useState(false);

  useEffect(() => {
    let script = document.createElement("script");
    script.src = THREE_JS_URL;
    script.async = true;
    document.body.appendChild(script);

    script.onload = () => {
      const scene = new THREE.Scene();
      scene.background = new THREE.Color(0x0a0a0a);
      scene.fog = new THREE.FogExp2(0x0a0a0a, 0.02);

      const camera = new THREE.PerspectiveCamera(75, 800 / 500, 0.1, 1000);
      const renderer = new THREE.WebGLRenderer({ antialias: true });
      renderer.setSize(800, 500);
      rendererRef.current = renderer;
      if (mountRef.current) mountRef.current.appendChild(renderer.domElement);

      const light = new THREE.DirectionalLight(0xffffff, 1);
      light.position.set(10, 20, 10);
      scene.add(light);
      scene.add(new THREE.AmbientLight(0x404040, 2.0));

      const geometry = new THREE.BoxGeometry(1, 1, 1);
      const materials = {
        1: new THREE.MeshLambertMaterial({ color: 0x54ba45 }),
        2: new THREE.MeshLambertMaterial({ color: 0x78350f }),
        3: new THREE.MeshLambertMaterial({ color: 0x64748b }),
        4: new THREE.MeshLambertMaterial({ color: 0x5d4037 }),
        5: new THREE.MeshLambertMaterial({ color: 0x2e7d32 }),
      };

      const world = new THREE.Group();
      scene.add(world);

      const createBlock = (x, y, z, type) => {
        const mesh = new THREE.Mesh(geometry, materials[type]);
        mesh.position.set(x, y, z);
        mesh.userData.type = type;
        world.add(mesh);
      };

      const size = 18;
      for (let x = -size; x < size; x++) {
        for (let z = -size; z < size; z++) {
          const h = Math.floor(Math.sin(x * 0.15) * 2 + Math.cos(z * 0.15) * 2);
          for (let y = -4; y <= h; y++) {
            let type = 3;
            if (y === h) type = 1;
            else if (y > h - 3) type = 2;
            createBlock(x, y, z, type);
          }
          if (x % 8 === 0 && z % 8 === 0 && Math.random() > 0.3) {
            const treeBaseH = h + 1;
            for(let th = 0; th < 4; th++) createBlock(x, treeBaseH + th, z, 4);
            for(let lx = -1; lx <= 1; lx++) {
              for(let lz = -1; lz <= 1; lz++) {
                for(let ly = 3; ly <= 4; ly++) {
                  if(lx === 0 && lz === 0 && ly === 3) continue;
                  createBlock(x + lx, treeBaseH + ly, z + lz, 5);
                }
              }
            }
            createBlock(x, treeBaseH + 5, z, 5);
          }
        }
      }

      camera.position.set(0, 8, 15);
      const keys = {};
      const onKeyDown = (e) => { 
        keys[e.code] = true; 
        if(e.code.startsWith('Digit')) {
          const val = parseInt(e.key);
          if(val >= 1 && val <= 5) setSelected(val);
        }
      };
      const onKeyUp = (e) => keys[e.code] = false;
      document.addEventListener('keydown', onKeyDown);
      document.addEventListener('keyup', onKeyUp);

      const euler = new THREE.Euler(0, 0, 0, 'YXZ');
      const onMouseMove = (e) => {
        if (!document.pointerLockElement) return;
        euler.y -= e.movementX * 0.002;
        euler.x -= e.movementY * 0.002;
        euler.x = Math.max(-Math.PI / 2, Math.min(Math.PI / 2, euler.x));
        camera.quaternion.setFromEuler(euler);
      };
      document.addEventListener('mousemove', onMouseMove);

      const raycaster = new THREE.Raycaster();
      const mouse = new THREE.Vector2(0, 0);

      const handleInteraction = (e) => {
        if (!document.pointerLockElement) {
          renderer.domElement.requestPointerLock();
          return;
        }
        raycaster.setFromCamera(mouse, camera);
        const intersects = raycaster.intersectObjects(world.children);
        if (intersects.length > 0) {
          const intersect = intersects[0];
          if (e.button === 0) world.remove(intersect.object);
          else if (e.button === 2) {
            const pos = intersect.object.position.clone().add(intersect.face.normal);
            createBlock(pos.x, pos.y, pos.z, selected);
          }
        }
      };
      renderer.domElement.addEventListener('mousedown', handleInteraction);

      const animate = () => {
        if (!rendererRef.current) return;
        requestAnimationFrame(animate);
        const speed = 0.15;
        const dir = new THREE.Vector3();
        camera.getWorldDirection(dir);
        dir.y = 0; dir.normalize();
        const side = new THREE.Vector3().crossVectors(camera.up, dir).normalize();

        if (keys['KeyW']) camera.position.addScaledVector(dir, speed);
        if (keys['KeyS']) camera.position.addScaledVector(dir, -speed);
        if (keys['KeyA']) camera.position.addScaledVector(side, speed);
        if (keys['KeyD']) camera.position.addScaledVector(side, -speed);
        if (keys['Space']) camera.position.y += 0.12;
        if (keys['ShiftLeft']) camera.position.y -= 0.12;

        renderer.render(scene, camera);
      };
      animate();
      setLoaded(true);

      const onLockChange = () => setIsLocked(!!document.pointerLockElement);
      document.addEventListener('pointerlockchange', onLockChange);

      return () => {
        if (rendererRef.current) {
          rendererRef.current.dispose();
          rendererRef.current = null;
        }
        document.removeEventListener('keydown', onKeyDown);
        document.removeEventListener('keyup', onKeyUp);
        document.removeEventListener('mousemove', onMouseMove);
        document.removeEventListener('pointerlockchange', onLockChange);
      };
    };
  }, [selected]);

  return (
    <div className="relative w-full h-full flex flex-col items-center bg-black">
      <div ref={mountRef} className="w-full h-full cursor-pointer" 
           onClick={() => rendererRef.current?.domElement.requestPointerLock()} />
      
      {!isLocked && loaded && (
        <div className="absolute inset-0 bg-black/80 backdrop-blur-md flex flex-col items-center justify-center text-center p-6 z-10">
          <div className="bg-emerald-500/20 p-6 rounded-full mb-6 border border-emerald-500/30">
            <Play className="text-emerald-400 fill-emerald-400" size={48} />
          </div>
          <h3 className="text-4xl font-black mb-3 tracking-tighter text-white uppercase italic">Local Voxel</h3>
          <p className="text-neutral-400 text-base max-w-sm font-medium mb-8">
            Click anywhere to activate First-Person controls.
          </p>
        </div>
      )}

      {isLocked && (
        <div className="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 pointer-events-none opacity-80 z-20">
          <div className="w-6 h-[2px] bg-white absolute -left-3 top-0 shadow-lg" />
          <div className="h-6 w-[2px] bg-white absolute left-0 -top-3 shadow-lg" />
        </div>
      )}

      <div className="absolute bottom-8 flex gap-3 bg-black/60 p-4 rounded-3xl backdrop-blur-2xl border border-white/10 shadow-2xl z-20">
        {[1, 2, 3, 4, 5].map((id) => (
          <button key={id} onClick={(e) => { e.stopPropagation(); setSelected(id); }}
            className={`w-14 h-14 rounded-2xl flex flex-col items-center justify-center transition-all ${selected === id ? 'bg-emerald-500 scale-110' : 'bg-neutral-800 opacity-80'}`}>
            <div className="w-8 h-8 rounded-lg" style={{ 
              backgroundColor: id === 1 ? '#54ba45' : id === 2 ? '#78350f' : id === 3 ? '#64748b' : id === 4 ? '#5d4037' : '#2e7d32' 
            }} />
          </button>
        ))}
      </div>
    </div>
  );
};

// --- Neon Dash Component ---
const NeonDash = () => {
  const canvasRef = useRef(null);
  const [gameState, setGS] = useState(0);

  useEffect(() => {
    if (gameState !== 1) return;
    const ctx = canvasRef.current.getContext("2d");
    const p = { x: 100, y: 318, dy: 0, rot: 0, g: 1 };
    let obs = [], frame = 0, active = true;

    const loop = () => {
      if (!active) return;
      frame++;
      ctx.fillStyle = "#020617"; ctx.fillRect(0, 0, 800, 400);
      ctx.fillStyle = "#0f172a"; ctx.fillRect(0, 350, 800, 50); ctx.fillRect(0, 0, 800, 50);
      p.dy += 0.55 * p.g; p.y += p.dy;
      if (p.y > 318) { p.y = 318; p.dy = 0; p.rot = Math.round(p.rot/90)*90; }
      else if (p.y < 50) { p.y = 50; p.dy = 0; p.rot = Math.round(p.rot/90)*90; } 
      else p.rot += 5 * p.g;
      ctx.save(); ctx.translate(p.x+16, p.y+16); ctx.rotate(p.rot*Math.PI/180);
      ctx.fillStyle = "#f59e0b"; ctx.fillRect(-16, -16, 32, 32); ctx.restore();

      if (frame % 60 === 0) obs.push({ x: 800, y: p.g===1?318:50, w: 32, h: 32 });
      for (let i = obs.length - 1; i >= 0; i--) {
        obs[i].x -= 7;
        ctx.fillStyle = "#ef4444"; ctx.fillRect(obs[i].x, obs[i].y, obs[i].w, obs[i].h);
        if (p.x < obs[i].x + 32 && p.x + 32 > obs[i].x && p.y < obs[i].y + 32 && p.y + 32 > obs[i].y) {
          active = false; setGS(2);
        }
        if (obs[i].x < -50) obs.splice(i, 1);
      }
      requestAnimationFrame(loop);
    };
    const jump = () => (p.y >= 315 && p.g === 1 || p.y <= 55 && p.g === -1) && (p.dy = -9.5 * p.g);
    window.addEventListener("keydown", jump);
    loop();
    return () => { active = false; window.removeEventListener("keydown", jump); };
  }, [gameState]);

  return (
    <div className="relative w-full h-full bg-black flex items-center justify-center">
      <canvas ref={canvasRef} width={800} height={400} className="w-full h-full object-contain" />
      {gameState !== 1 && (
        <div className="absolute inset-0 bg-slate-950/90 flex flex-col items-center justify-center">
          <button onClick={() => setGS(1)} className="bg-white text-black px-12 py-4 rounded-2xl font-black text-xl hover:bg-amber-500">
            {gameState === 0 ? "START NEON DASH" : "REBOOT"}
          </button>
        </div>
      )}
    </div>
  );
};

// --- External Game Component (Roblox / Bloxd) ---
const ExternalGame = ({ url }) => (
  <div className="w-full h-full bg-neutral-900 flex flex-col">
    <div className="bg-black/80 px-4 py-2 flex items-center justify-between border-b border-white/10">
      <div className="flex items-center gap-2 text-[10px] font-mono text-white/40 uppercase tracking-widest">
        <Globe size={12}/> Secure_Portal: {url}
      </div>
      <a href={url} target="_blank" rel="noopener noreferrer" className="text-emerald-400 hover:text-white transition-colors">
        <ExternalLink size={14}/>
      </a>
    </div>
    <iframe src={url} className="flex-1 w-full border-none" title="external-game" />
  </div>
);

// --- Main App ---
export default function App() {
  const [activeGame, setActiveGame] = useState('menu');

  const games = [
    { id: 'minecraft', name: 'Voxel Engine', icon: <Box size={24}/>, color: 'bg-emerald-500', desc: 'Custom built 3D world sandbox.' },
    { id: 'neondash', name: 'Neon Dash', icon: <Zap size={24}/>, color: 'bg-amber-500', desc: 'High-speed geometric platforming.' },
    { id: 'roblox', name: 'Roblox', icon: <Monitor size={24}/>, color: 'bg-red-500', desc: 'Explore millions of 3D experiences.', url: 'https://www.roblox.com' },
    { id: 'bloxd', name: 'Bloxd.io', icon: <Pickaxe size={24}/>, color: 'bg-blue-500', desc: 'Multiplayer voxel building & games.', url: 'https://bloxd.io' }
  ];

  return (
    <div className="min-h-screen bg-[#050505] text-white font-sans flex flex-col items-center overflow-x-hidden selection:bg-emerald-500">
      <div className="absolute inset-0 pointer-events-none opacity-20">
        <div className="absolute inset-0 bg-[linear-gradient(to_right,#80808012_1px,transparent_1px),linear-gradient(to_bottom,#80808012_1px,transparent_1px)] bg-[size:40px_40px]"></div>
      </div>

      <header className="w-full max-w-7xl py-8 px-8 flex flex-col md:flex-row items-center justify-between gap-8 z-10">
        <div className="flex items-center gap-5 cursor-pointer" onClick={() => setActiveGame('menu')}>
          <div className="w-14 h-14 bg-white rounded-2xl flex items-center justify-center text-black shadow-xl">
            <Gamepad2 size={32} />
          </div>
          <h1 className="text-3xl font-black tracking-tighter uppercase italic">
            Soleminer<span className="text-emerald-500">Labs</span>
          </h1>
        </div>

        {activeGame !== 'menu' && (
          <button onClick={() => setActiveGame('menu')} className="bg-white/5 hover:bg-white/10 px-6 py-2 rounded-xl border border-white/10 text-xs font-black uppercase tracking-widest transition-all">
            Back to Menu
          </button>
        )}
      </header>

      <main className="w-full max-w-7xl px-8 pb-32 z-10 flex flex-col items-center">
        {activeGame === 'menu' ? (
          <div className="w-full">
            <h2 className="text-neutral-500 text-xs font-black uppercase tracking-[0.4em] mb-12 text-center">Select_Interactive_Environment</h2>
            <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
              {games.map((game) => (
                <div 
                  key={game.id} 
                  onClick={() => setActiveGame(game.id)}
                  className="bg-neutral-900/40 p-8 rounded-[2rem] border border-white/5 hover:border-white/20 hover:bg-neutral-900/60 transition-all duration-500 cursor-pointer group flex flex-col h-full"
                >
                  <div className={`${game.color} w-16 h-16 rounded-2xl flex items-center justify-center text-black mb-6 group-hover:scale-110 transition-transform shadow-lg`}>
                    {game.icon}
                  </div>
                  <h3 className="text-2xl font-black tracking-tight mb-3 group-hover:text-emerald-400 transition-colors uppercase italic">{game.name}</h3>
                  <p className="text-neutral-500 text-sm leading-relaxed mb-8 flex-grow">{game.desc}</p>
                  <div className="flex items-center gap-2 text-[10px] font-black uppercase tracking-widest opacity-30 group-hover:opacity-100 transition-opacity">
                    Initialize Core <ChevronRight size={12}/>
                  </div>
                </div>
              ))}
            </div>
          </div>
        ) : (
          <div className="relative aspect-video w-full max-w-6xl rounded-[3rem] p-2 bg-gradient-to-br from-white/10 to-transparent border border-white/10 shadow-2xl overflow-hidden bg-black">
            <div className="w-full h-full rounded-[2.6rem] overflow-hidden">
              {activeGame === 'minecraft' && <Minecraft3D />}
              {activeGame === 'neondash' && <NeonDash />}
              {activeGame === 'roblox' && <ExternalGame url="https://www.roblox.com" />}
              {activeGame === 'bloxd' && <ExternalGame url="https://bloxd.io" />}
            </div>
          </div>
        )}

        <div className="mt-20 grid grid-cols-1 md:grid-cols-3 gap-8 w-full max-w-5xl">
          <div className="bg-neutral-900/20 p-8 rounded-[2rem] border border-white/5">
            <h4 className="text-white/40 text-[10px] font-black uppercase tracking-widest mb-4">Core_System</h4>
            <p className="text-neutral-500 text-xs leading-relaxed">Cross-platform voxel engine compatible with legacy browsers and modern hardware.</p>
          </div>
          <div className="bg-neutral-900/20 p-8 rounded-[2rem] border border-white/5">
            <h4 className="text-white/40 text-[10px] font-black uppercase tracking-widest mb-4">Security</h4>
            <p className="text-neutral-500 text-xs leading-relaxed">Encapsulated game environments ensuring safe, age-appropriate content across all portals.</p>
          </div>
          <div className="bg-neutral-900/20 p-8 rounded-[2rem] border border-white/5">
            <h4 className="text-white/40 text-[10px] font-black uppercase tracking-widest mb-4">Connectivity</h4>
            <p className="text-neutral-500 text-xs leading-relaxed">Low-latency bridge to major external hubs including Roblox and Bloxd platforms.</p>
          </div>
        </div>
      </main>

      <footer className="w-full py-12 px-8 border-t border-white/5 bg-black/40 mt-auto">
        <div className="max-w-7xl mx-auto flex flex-col md:flex-row justify-between items-center gap-8 opacity-30">
          <div className="flex gap-8 text-[10px] font-black uppercase tracking-widest">
            <span>Status: Online</span>
            <span>Uptime: 99.9%</span>
          </div>
          <p className="text-[10px] font-mono tracking-widest uppercase">© 2026 SOLEMINER LABS • ACCESS_GRANTED</p>
        </div>
      </footer>
    </div>
  );
}
