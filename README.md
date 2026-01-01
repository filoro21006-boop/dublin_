<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="theme-color" content="#1e3932">
    <title>Dublin OS Starbucks</title>
    <style>
        /* =========================================
           1. DESIGN SYSTEM (STARBUCKS GREEN)
           ========================================= */
        :root {
            --sb-green: #00704A;       /* Iconic Green */
            --sb-dark-roast: #1e3932;  /* Dark Background */
            --sb-latte: #f2f0eb;       /* Text / Light */
            --sb-gold: #cba258;        /* Gold Stars */
            
            /* Glassmorphism Tinted Green */
            --glass-surface: rgba(30, 57, 50, 0.75); 
            --glass-border: 1px solid rgba(255, 255, 255, 0.1);
            --glass-blur: blur(25px);
            
            --ease-ios: cubic-bezier(0.32, 0.72, 0, 1);
        }

        * { box-sizing: border-box; -webkit-tap-highlight-color: transparent; outline: none; }

        /* FIX SCROLL IPHONE CRUCIALE */
        body {
            margin: 0; padding: 0;
            font-family: -apple-system, BlinkMacSystemFont, "SF Pro Display", sans-serif;
            background-color: #0b1210; /* Almost Black Green */
            color: white;
            /* Forza l'altezza esatta dello schermo visibile */
            height: 100vh; 
            height: 100dvh; 
            overflow: hidden; /* Blocca il rimbalzo della pagina intera */
            display: flex; flex-direction: column;
            user-select: none; -webkit-user-select: none;
        }

        /* SFONDO LIQUIDO VERDE */
        .ambient-layer {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%; z-index: -1;
            background: radial-gradient(circle at 50% 0%, var(--sb-dark-roast), #000);
        }
        .orb {
            position: absolute; border-radius: 50%; filter: blur(80px); opacity: 0.4;
            animation: float 20s infinite ease-in-out alternate;
        }
        .orb-1 { width: 350px; height: 350px; background: var(--sb-green); top: -10%; left: -20%; }
        .orb-2 { width: 300px; height: 300px; background: var(--sb-gold); bottom: 0; right: -20%; opacity: 0.2; }
        @keyframes float { 0%{transform:translate(0,0)} 100%{transform:translate(20px,-30px)} }

        /* LAYOUT PRINCIPALE */
        .app-layout {
            flex: 1; display: flex; flex-direction: column;
            height: 100%; position: relative; z-index: 10;
        }

        /* HEADER FISSO */
        .header {
            padding: 55px 24px 15px 24px;
            display: flex; justify-content: space-between; align-items: flex-end;
            background: linear-gradient(to bottom, rgba(11,18,16,0.9), transparent);
            flex-shrink: 0; z-index: 50;
        }
        .brand-sub { font-size: 11px; color: var(--sb-gold); font-weight: 700; letter-spacing: 2px; text-transform: uppercase; }
        .brand-main { font-size: 28px; font-weight: 800; letter-spacing: -0.5px; }
        .stars-pill {
            background: rgba(0, 112, 74, 0.3); border: 1px solid var(--sb-green);
            padding: 8px 16px; border-radius: 30px; display: flex; align-items: center; gap: 6px;
            backdrop-filter: blur(10px);
        }

        /* AREA SCROLLABILE (FIXED) */
        .scroll-container {
            flex: 1;
            overflow-y: auto; /* Abilita scroll */
            overflow-x: hidden;
            -webkit-overflow-scrolling: touch; /* Momentum scroll nativo iOS */
            padding: 10px 20px 140px 20px; /* Padding fondo extra per la Dock */
        }
        .scroll-container::-webkit-scrollbar { display: none; }

        /* CARDS & UI */
        .card {
            background: var(--glass-surface); backdrop-filter: var(--glass-blur); -webkit-backdrop-filter: var(--glass-blur);
            border: var(--glass-border); border-radius: 24px; padding: 22px; margin-bottom: 20px;
            box-shadow: 0 20px 40px -10px rgba(0,0,0,0.5); position: relative; transform: translateZ(0);
        }
        .card:active { transform: scale(0.98); transition: 0.1s; }

        /* Livello */
        .streak-badge {
            position: absolute; top: 20px; right: 20px;
            background: rgba(203, 162, 88, 0.2); color: var(--sb-gold);
            padding: 6px 12px; border-radius: 14px; font-size: 11px; font-weight: 800;
        }
        .level-num { font-size: 60px; font-weight: 900; line-height: 1; text-shadow: 0 0 30px rgba(0,112,74,0.6); }
        .progress-bar { height: 6px; background: rgba(255,255,255,0.1); border-radius: 10px; overflow: hidden; margin-top: 15px; }
        .progress-fill { height: 100%; background: var(--sb-green); width: 0%; transition: width 0.5s var(--ease-ios); box-shadow: 0 0 10px var(--sb-green); }

        /* Tasks */
        .label-sec { font-size: 12px; font-weight: 800; color: var(--sb-gold); text-transform: uppercase; letter-spacing: 1px; margin: 25px 0 10px 5px; opacity: 0.8; }
        
        .task-row { display: flex; justify-content: space-between; align-items: center; padding: 16px 0; border-bottom: 1px solid rgba(255,255,255,0.05); }
        .task-row:last-child { border-bottom: none; }
        .icon { width: 44px; height: 44px; background: rgba(0,112,74,0.2); border-radius: 14px; display: flex; align-items: center; justify-content: center; font-size: 22px; margin-right: 15px; color: var(--sb-green); border: 1px solid rgba(0,112,74,0.3); }
        .btn-act { background: var(--sb-green); color: white; border: none; padding: 8px 14px; border-radius: 20px; font-size: 11px; font-weight: 700; }
        .task-row.done { opacity: 0.4; pointer-events: none; }
        .task-row.done .btn-act { background: #333; content: "DONE"; }

        /* Grid */
        .grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
        .tile { background: var(--glass-surface); border: var(--glass-border); border-radius: 20px; padding: 15px; text-align: center; }
        .tile:active { background: rgba(255,255,255,0.1); }
        .tile.tech { border: 1px solid var(--sb-gold); background: linear-gradient(135deg, rgba(203,162,88,0.15), transparent); }

        /* Wallet */
        .wallet-card { background: linear-gradient(135deg, #05110e, #1e3932); border: 1px solid rgba(255,255,255,0.1); min-height: 200px; display: flex; flex-direction: column; justify-content: space-between; position: relative; overflow: hidden; }
        .wallet-card::before { content: ''; position: absolute; width: 150px; height: 150px; background: var(--sb-green); filter: blur(60px); opacity: 0.2; top: -20px; right: -20px; }
        
        /* Dock (Navigazione) */
        .dock {
            position: fixed; bottom: 30px; left: 50%; transform: translateX(-50%);
            background: rgba(5, 17, 14, 0.95); backdrop-filter: blur(30px); -webkit-backdrop-filter: blur(30px);
            border: 1px solid rgba(255,255,255,0.15); padding: 14px 35px; border-radius: 40px;
            display: flex; gap: 40px; align-items: center; box-shadow: 0 20px 50px rgba(0,0,0,0.8); z-index: 1000;
        }
        .dock-ico { font-size: 26px; color: rgba(255,255,255,0.4); transition: 0.3s; position: relative; }
        .dock-ico.active { color: var(--sb-green); transform: translateY(-5px); text-shadow: 0 0 15px var(--sb-green); }
        .dock-dot { width: 4px; height: 4px; background: var(--sb-green); border-radius: 50%; position: absolute; bottom: -10px; opacity: 0; }
        .dock-ico.active .dock-dot { opacity: 1; }

        /* Utils */
        .view { display: none; animation: fade 0.3s ease; }
        .view.active { display: block; }
        @keyframes fade { from{opacity:0; transform:translateY(10px);} to{opacity:1; transform:translateY(0);} }
        
        #toast { position: fixed; top: 20px; left: 50%; transform: translateX(-50%) translateY(-100px); background: var(--sb-green); color: white; padding: 12px 24px; border-radius: 30px; font-weight: 700; font-size: 13px; z-index: 2000; transition: 0.4s var(--ease-ios); box-shadow: 0 10px 20px rgba(0,0,0,0.3); }
        #toast.show { transform: translateX(-50%) translateY(0); }
        
        .part { position: absolute; width: 6px; height: 6px; border-radius: 50%; pointer-events: none; animation: pop 0.8s forwards; }
        @keyframes pop { to { transform: translate(var(--dx), var(--dy)) scale(0); opacity: 0; } }

    </style>
</head>
<body>

    <div class="ambient-layer">
        <div class="orb orb-1"></div>
        <div class="orb orb-2"></div>
    </div>

    <div class="app-layout">
        
        <div class="header">
            <div>
                <div class="brand-sub">DUBLIN OS</div>
                <div class="brand-main">Starbucks HQ</div>
            </div>
            <div class="stars-pill">
                <span style="color:var(--sb-gold);">★</span>
                <span style="font-weight:800;" id="starDisplay">0</span>
            </div>
        </div>

        <div class="scroll-container">

            <div id="v-home" class="view active">
                
                <div style="display:flex; gap:10px; margin-bottom:20px;">
                    <div class="card" style="flex:1; padding:15px; margin:0; text-align:center;">
                        <div style="font-size:10px; color:var(--sb-gold); font-weight:700; margin-bottom:5px;">COUNTDOWN</div>
                        <div id="cdDisplay" style="font-weight:800; font-size:16px;">Loading...</div>
                    </div>
                    <div class="card" style="flex:1; padding:15px; margin:0; text-align:center;">
                        <div style="font-size:10px; color:var(--sb-green); font-weight:700; margin-bottom:5px;">DUBLIN TIME</div>
                        <div id="timeDisplay" style="font-weight:800; font-size:16px;">00:00</div>
                    </div>
                </div>

                <div class="card">
                    <div class="streak-badge">🔥 <span id="streakDisplay">0</span></div>
                    <div class="level-num" id="levelDisplay">1</div>
                    <div style="font-size:12px; color:var(--sb-green); font-weight:700; text-transform:uppercase; letter-spacing:1px; margin-bottom:10px;" id="rankDisplay">GREEN MEMBER</div>
                    <div style="display:flex; justify-content:space-between; font-size:11px; opacity:0.6;">
                        <span>Progress</span><span><span id="xpDisplay">0</span> / 100</span>
                    </div>
                    <div class="progress-bar"><div class="progress-fill" id="xpBar"></div></div>
                </div>

                <div class="label-sec">Daily Brew</div>
                <div class="card" style="padding: 10px 24px;">
                    <div class="task-row" id="t1" onclick="act('t1', 25, 10)">
                        <div style="display:flex; align-items:center;">
                            <div class="icon">🤖</div>
                            <div><div style="font-weight:700;">AI Content</div><div style="font-size:11px; opacity:0.6;">Create & Post</div></div>
                        </div>
                        <button class="btn-act">+10★</button>
                    </div>
                    <div class="task-row" id="t2" onclick="act('t2', 30, 15)">
                        <div style="display:flex; align-items:center;">
                            <div class="icon">📹</div>
                            <div><div style="font-weight:700;">Vlog Clip</div><div style="font-size:11px; opacity:0.6;">Record Daily</div></div>
                        </div>
                        <button class="btn-act">+15★</button>
                    </div>
                    <div class="task-row" id="t3" onclick="act('t3', 15, 5)">
                        <div style="display:flex; align-items:center;">
                            <div class="icon">🇬🇧</div>
                            <div><div style="font-weight:700;">English</div><div style="font-size:11px; opacity:0.6;">10m Practice</div></div>
                        </div>
                        <button class="btn-act">+5★</button>
                    </div>
                </div>

                <div class="label-sec">Extra Shots</div>
                <div class="grid">
                    <div class="tile" id="s1" onclick="act('s1', 10, 2)">
                        <div style="font-size:24px; margin-bottom:5px;">🏋️</div><div style="font-size:12px; font-weight:700;">Gym</div><div style="font-size:10px; color:var(--sb-gold);">+2★</div>
                    </div>
                    <div class="tile" id="s2" onclick="act('s2', 5, 2)">
                        <div style="font-size:24px; margin-bottom:5px;">📚</div><div style="font-size:12px; font-weight:700;">Read</div><div style="font-size:10px; color:var(--sb-gold);">+2★</div>
                    </div>
                    <div class="tile" id="s3" onclick="act('s3', 10, 5)">
                        <div style="font-size:24px; margin-bottom:5px;">💰</div><div style="font-size:12px; font-weight:700;">Save</div><div style="font-size:10px; color:var(--sb-gold);">+5★</div>
                    </div>
                    <div class="tile" id="s4" onclick="act('s4', 5, 2)">
                        <div style="font-size:24px; margin-bottom:5px;">🧹</div><div style="font-size:12px; font-weight:700;">Clean</div><div style="font-size:10px; color:var(--sb-gold);">+2★</div>
                    </div>
                </div>

                <div style="text-align:center; padding:30px; opacity:0.5;">
                    <button onclick="resetDay()" style="background:none; border:none; color:white; font-weight:700; letter-spacing:1px; cursor:pointer; font-size:10px;">TAP FOR NEW DAY</button>
                </div>
            </div>

            <div id="v-wallet" class="view">
                <div class="card wallet-card">
                    <div style="display:flex; justify-content:space-between; align-items:center;">
                        <span style="font-size:12px; font-weight:800; color:var(--sb-gold); letter-spacing:1px;">DUBLIN CARD</span>
                        <span style="font-size:24px;">🇮🇪</span>
                    </div>
                    <div style="z-index:2;">
                        <div style="font-size:11px; opacity:0.6; font-weight:700; margin-bottom:5px;">TOTAL BALANCE</div>
                        <div style="font-size:42px; font-weight:800;">€<span id="moneyDisplay">0.00</span></div>
                    </div>
                </div>

                <div class="grid" style="margin-bottom:30px;">
                    <div class="tile" onclick="money(1)" style="border:1px solid var(--sb-green);">
                        <div style="font-size:20px; color:var(--sb-green); margin-bottom:5px;">+</div>
                        <div style="font-size:12px; font-weight:700; color:var(--sb-green);">DEPOSIT</div>
                    </div>
                    <div class="tile" onclick="money(-1)" style="border:1px solid #ff453a;">
                        <div style="font-size:20px; color:#ff453a; margin-bottom:5px;">-</div>
                        <div style="font-size:12px; font-weight:700; color:#ff453a;">PAYMENT</div>
                    </div>
                </div>

                <div class="label-sec">History</div>
                <div id="txList" style="display:flex; flex-direction:column; gap:10px;"></div>
            </div>

            <div id="v-shop" class="view">
                
                <div class="card" style="background:rgba(0, 112, 74, 0.2); border-color:var(--sb-green);">
                    <div style="font-size:12px; font-weight:800; color:var(--sb-green); margin-bottom:10px;">YOUR TRAY</div>
                    <div id="invList" style="text-align:center; font-size:13px; opacity:0.7;">Empty.</div>
                </div>

                <div class="label-sec">Starbucks Rewards</div>
                <div class="grid">
                    <div class="tile" onclick="buy(10, 'Espresso Shot ☕️')">
                        <div style="font-size:24px; margin-bottom:5px;">☕️</div><div style="font-size:12px; font-weight:700;">Espresso</div><div style="font-size:10px; opacity:0.7;">10★</div>
                    </div>
                    <div class="tile" onclick="buy(20, 'Music 1h 🎵')">
                        <div style="font-size:24px; margin-bottom:5px;">🎵</div><div style="font-size:12px; font-weight:700;">Music 1h</div><div style="font-size:10px; opacity:0.7;">20★</div>
                    </div>
                    <div class="tile" onclick="buy(50, 'Break 15m 🧘')">
                        <div style="font-size:24px; margin-bottom:5px;">🧘</div><div style="font-size:12px; font-weight:700;">Break 15m</div><div style="font-size:10px; opacity:0.7;">50★</div>
                    </div>
                    <div class="tile" onclick="buy(80, 'Netflix Ep 🎬')">
                        <div style="font-size:24px; margin-bottom:5px;">🎬</div><div style="font-size:12px; font-weight:700;">Netflix Ep</div><div style="font-size:10px; opacity:0.7;">80★</div>
                    </div>
                    <div class="tile" onclick="buy(150, 'Cheat Meal 🍔')">
                        <div style="font-size:24px; margin-bottom:5px;">🍔</div><div style="font-size:12px; font-weight:700;">Cheat Meal</div><div style="font-size:10px; opacity:0.7;">150★</div>
                    </div>
                    <div class="tile" onclick="buy(200, 'Cinema 🍿')">
                        <div style="font-size:24px; margin-bottom:5px;">🍿</div><div style="font-size:12px; font-weight:700;">Cinema</div><div style="font-size:10px; opacity:0.7;">200★</div>
                    </div>
                    <div class="tile" onclick="buy(300, 'Gold Status ⭐️')">
                        <div style="font-size:24px; margin-bottom:5px;">⭐️</div><div style="font-size:12px; font-weight:700;">Gold Status</div><div style="font-size:10px; opacity:0.7;">300★</div>
                    </div>
                </div>
            </div>

        </div>
    </div>

    <div class="dock">
        <div class="dock-ico active" id="n-home" onclick="tab('home')">🏠<div class="dock-dot"></div></div>
        <div class="dock-ico" id="n-wallet" onclick="tab('wallet')">💳<div class="dock-dot"></div></div>
        <div class="dock-ico" id="n-shop" onclick="tab('shop')">🛍️<div class="dock-dot"></div></div>
    </div>

    <div id="toast">✅ Done</div>

<script>
    /* ENGINE DEFINITIVO */
    const DB = 'dublin_os_pure_green';
    let s = { xp:0, stars:0, money:0, streak:0, lastLogin:'', done:[], inv:[], hist:[] };

    function init() {
        let d = localStorage.getItem(DB);
        if(d) s = JSON.parse(d);
        else s.lastLogin = new Date().toDateString();
        
        checkStreak();
        setInterval(updateTimers, 1000);
        updateTimers();
        render();
    }

    /* LOGICA */
    function act(id, x, st) {
        if(s.done.includes(id)) return;
        fx(event.clientX, event.clientY); notify(`+${st} Stars`);
        s.xp+=x; s.stars+=st; s.done.push(id); save(); render();
    }

    function buy(cost, name) {
        if(s.stars >= cost) {
            s.stars -= cost; s.inv.push(name);
            fx(event.clientX, event.clientY); notify('Added to Tray');
            save(); render();
        } else notify('Not enough Stars');
    }

    function use(idx) {
        s.inv.splice(idx, 1); notify('Redeemed!'); save(); render();
    }

    function money(m) {
        let v = parseFloat(prompt(m > 0 ? "Deposit €:" : "Pay €:"));
        if(v > 0) {
            s.money += (v * m);
            s.hist.unshift({ t: m > 0 ? "Deposit" : "Payment", v: v, d: new Date().toLocaleDateString() });
            save(); render();
        }
    }

    function resetDay() {
        s.done = []; notify('New Day'); save();
        setTimeout(() => location.reload(), 400);
    }

    /* RENDER SYSTEM */
    function render() {
        // Stats
        document.getElementById('starDisplay').innerText = s.stars;
        let lvl = Math.floor(s.xp / 100) + 1;
        document.getElementById('levelDisplay').innerText = lvl;
        document.getElementById('xpDisplay').innerText = s.xp % 100;
        document.getElementById('xpBar').style.width = (s.xp % 100) + "%";
        document.getElementById('streakDisplay').innerText = s.streak;
        document.getElementById('moneyDisplay').innerText = s.money.toLocaleString('en-IE', {minimumFractionDigits: 2});
        
        let ranks = ["Green Level", "Gold Level", "Platinum", "Diamond", "Dublin Elite"];
        document.getElementById('rankDisplay').innerText = ranks[Math.min(lvl - 1, 4)];

        // Task States
        s.done.forEach(id => {
            let el = document.getElementById(id);
            if(el) el.classList.add('done');
        });

        // Inventory
        let invH = "";
        if(s.inv.length == 0) invH = "Tray is empty.";
        else s.inv.forEach((i, x) => invH += `<div style='display:flex; justify-content:space-between; background:rgba(255,255,255,0.05); padding:12px; border-radius:12px; margin-bottom:8px;'><span>${i}</span><span onclick='use(${x})' style='color:var(--sb-green); font-weight:800; cursor:pointer;'>USE</span></div>`);
        document.getElementById('invList').innerHTML = invH;

        // Transactions
        let txH = "";
        s.hist.slice(0,10).forEach(h => txH += `<div style='display:flex; justify-content:space-between; background:rgba(255,255,255,0.05); padding:12px; border-radius:12px; margin-bottom:8px; font-size:13px;'><span>${h.t} <span style='opacity:0.5'>${h.d}</span></span><span style='color:${h.t=="Deposit"?"var(--sb-green)":"#ff453a"}'>€${h.v}</span></div>`);
        document.getElementById('txList').innerHTML = txH;
    }

    /* TIMERS */
    function updateTimers() {
        // Countdown Nov 2026
        const target = new Date("2026-11-01T00:00:00").getTime();
        const diff = target - new Date().getTime();
        const days = Math.floor(diff / (1000*60*60*24));
        document.getElementById('cdDisplay').innerText = diff > 0 ? `${days} Days` : "ARRIVED";

        // Dublin Time
        const dublin = new Date().toLocaleTimeString("en-IE", { timeZone: "Europe/Dublin", hour:'2-digit', minute:'2-digit', hour12:false });
        document.getElementById('timeDisplay').innerText = dublin;
    }

    /* UTILS */
    function tab(t) {
        document.querySelectorAll('.view').forEach(e => e.classList.remove('active'));
        document.querySelectorAll('.dock-ico').forEach(e => e.classList.remove('active'));
        document.getElementById('v-'+t).classList.add('active');
        document.getElementById('n-'+t).classList.add('active');
    }

    function notify(m) {
        let t = document.getElementById('toast');
        t.innerText = m; t.classList.add('show');
        if(navigator.vibrate) navigator.vibrate(15);
        setTimeout(() => t.classList.remove('show'), 2000);
    }

    function fx(x, y) {
        for(let i=0; i<20; i++) {
            let p = document.createElement('div'); p.className = 'part';
            document.body.appendChild(p);
            p.style.left = x+'px'; p.style.top = y+'px';
            p.style.background = ['#00704A', '#cba258'][Math.floor(Math.random()*2)];
            let a = Math.random()*6.28, v = Math.random()*150;
            p.style.setProperty('--dx', Math.cos(a)*v+'px');
            p.style.setProperty('--dy', Math.sin(a)*v+'px');
            setTimeout(() => p.remove(), 800);
        }
    }

    function checkStreak() {
        let t = new Date().toDateString();
        if(s.lastLogin != t) {
            let y = new Date(); y.setDate(y.getDate() - 1);
            s.streak = (s.lastLogin == y.toDateString()) ? s.streak + 1 : 1;
            s.lastLogin = t; save();
        }
    }

    function save() { localStorage.setItem(DB, JSON.stringify(s)); }

    init();
</script>
</body>
</html>
