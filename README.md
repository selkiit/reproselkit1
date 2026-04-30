<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Radio Selkit | En Vivo</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=DM+Sans:ital,wght@0,300;0,500;0,700;1,300&family=DM+Mono:wght@400;500;700&display=swap" rel="stylesheet">
    <style>
        *, *::before, *::after {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            -webkit-tap-highlight-color: transparent;
        }

        :root {
            --pk:        #A855F7; 
            --pk-glow:   rgba(168, 85, 247, 0.6);
            --player-bg: #1e1e1e; 
            --border-color: rgba(255, 255, 255, 0.08);
            --text:      #FFFFFF;
            --muted:     rgba(255, 255, 255, 0.5);
            --r-xl:      32px;
        }

        html, body {
            height: 100%;
            background: #121212;
            color: var(--text);
            font-family: 'DM Sans', sans-serif;
            overflow: hidden; 
        }

        body {
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 20px;
        }

        #dynbg {
            position: fixed;
            inset: 0;
            background-size: cover;
            background-position: center;
            filter: blur(80px) grayscale(0.8) brightness(0.15);
            transform: scale(1.1);
            z-index: 0;
            transition: background-image 2.5s ease;
        }

        .shell {
            position: relative;
            z-index: 1;
            width: 100%;
            max-width: 380px;
            display: flex;
            flex-direction: column;
            gap: 1rem;
        }

        .top-bar {
            display: flex;
            justify-content: center;
            padding: 0 5px;
            margin-bottom: 5px;
        }

        .brand-logo {
            max-width: 100%;
            height: auto;
            max-height: 80px;
            filter: drop-shadow(0 0 10px rgba(0, 0, 0, 0.5));
        }

        .glass-card {
            background: var(--player-bg);
            border: 1px solid var(--border-color);
            border-radius: var(--r-xl);
            box-shadow: 0 40px 80px rgba(0, 0, 0, 0.6);
            overflow: hidden;
            display: flex;
            flex-direction: column;
            position: relative;
        }

        /* Historial */
        #historyPanel {
            position: absolute;
            inset: 0;
            background: rgba(30, 30, 30, 0.98);
            backdrop-filter: blur(20px);
            z-index: 100;
            padding: 20px;
            display: none;
            flex-direction: column;
            overflow-y: auto;
        }
        #historyPanel.open { display: flex; }
        .history-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            padding-bottom: 12px;
            border-bottom: 1px solid var(--border-color);
        }
        .history-title { font-weight: 700; font-size: 16px; color: var(--pk); letter-spacing: 1px; }
        .close-history { cursor: pointer; color: var(--muted); font-size: 11px; font-weight: 800; text-transform: uppercase; }

        .history-item {
            display: flex;
            align-items: center;
            gap: 14px;
            padding: 12px 0;
            border-bottom: 1px solid rgba(255, 255, 255, 0.03);
            animation: fadeIn 0.4s ease forwards;
        }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(5px); } to { opacity: 1; transform: translateY(0); } }

        .h-art { width: 44px; height: 44px; border-radius: 8px; object-fit: cover; background: #333; }
        .h-info { flex: 1; overflow: hidden; }
        .h-tit { font-size: 13px; font-weight: 700; color: #fff; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; margin-bottom: 2px; }
        .h-artst { font-size: 11px; color: var(--muted); }

        /* Cabecera Meta */
        .header-meta {
            display: flex;
            justify-content: space-between;
            align-items: flex-start;
            padding: 1.5rem;
            border-bottom: 1px solid var(--border-color);
            background: rgba(255, 255, 255, 0.02);
        }

        .header-left {
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .dj-avatar-container {
            width: 48px;
            height: 48px;
            border-radius: 12px;
            background: #2a2a2a;
            border: 1px solid var(--border-color);
            overflow: hidden;
            display: flex;
            align-items: center;
            justify-content: center;
            flex-shrink: 0;
        }

        .dj-avatar-container.active { border-color: var(--pk); }
        #djImg { width: 100%; height: 100%; object-fit: cover; display: none; }
        #djInitial { font-weight: 800; font-size: 16px; color: var(--muted); }

        .dj-info-mini {
            display: flex;
            flex-direction: column;
            align-items: flex-start;
        }

        .dj-label { 
            font-size: 9px; 
            text-transform: uppercase; 
            letter-spacing: 0.1em; 
            color: var(--muted);
            font-weight: 700;
            display: flex;
            align-items: center;
            gap: 4px;
        }

        .dj-name-txt {
            font-weight: 700; 
            font-size: 14px; 
            color: #fff;
            max-width: 120px;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }

        .header-right {
            text-align: right;
            display: flex;
            flex-direction: column;
            align-items: flex-end;
            gap: 4px;
        }

        .card-clock {
            display: flex;
            align-items: baseline;
            gap: 3px;
        }
        #clockTime {
            font-size: 24px;
            font-weight: 300;
            line-height: 1;
            color: #fff;
        }
        #clockAmPm {
            font-size: 10px;
            font-weight: 800;
            color: var(--pk);
            text-transform: uppercase;
        }

        .listeners-mini {
            font-size: 9px;
            font-weight: 700;
            color: var(--muted);
            letter-spacing: 0.05em;
        }
        #listenersCount { color: #fff; }

        .history-trigger {
            margin-top: 6px;
            background: rgba(255, 255, 255, 0.05);
            border: 1px solid var(--border-color);
            width: 32px;
            height: 32px;
            border-radius: 8px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        .history-trigger:hover { background: var(--pk); border-color: var(--pk); }
        .history-trigger svg { width: 16px; height: 16px; fill: var(--muted); transition: fill 0.3s; }
        .history-trigger:hover svg { fill: #fff; }

        /* Vinilo */
        .vinyl-area {
            padding: 2.5rem 0;
            display: flex;
            justify-content: center;
            background: radial-gradient(circle at center, rgba(255,255,255,0.03) 0%, transparent 70%);
        }

        .vinyl-ring {
            width: 180px;
            height: 180px;
            border-radius: 50%;
            background: #111;
            padding: 6px;
            border: 4px solid #282828;
            box-shadow: 0 20px 40px rgba(0,0,0,0.5);
            position: relative;
        }

        .vinyl-img {
            width: 100%;
            height: 100%;
            border-radius: 50%;
            object-fit: cover;
            opacity: 0.9;
        }

        .spinning { animation: rotate 20s linear infinite; }
        @keyframes rotate { from { transform: rotate(0deg); } to { transform: rotate(360deg); } }

        /* Track */
        .track-info {
            padding: 0 1.5rem 1.5rem;
            text-align: center;
        }

        .marquee { overflow: hidden; white-space: nowrap; margin-bottom: 6px; }
        .marquee-inner {
            display: inline-block;
            padding-left: 100%;
            animation: scroll 14s linear infinite;
            font-size: 20px;
            font-weight: 700;
            color: #eee;
        }
        @keyframes scroll { 0% { transform: translate(0, 0); } 100% { transform: translate(-100%, 0); } }

        .artist-name { color: var(--pk); font-size: 13px; font-weight: 500; letter-spacing: 0.02em; }

        /* Controles */
        .player-controls {
            display: flex;
            align-items: center;
            padding: 0 1.5rem 2.5rem;
            gap: 1.2rem;
        }

        .main-play {
            width: 60px;
            height: 60px;
            border-radius: 50%;
            background: var(--pk);
            border: none;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0 8px 20px var(--pk-glow);
            cursor: pointer;
            flex-shrink: 0;
            transition: transform 0.2s;
        }
        .main-play:hover { transform: scale(1.05); }
        .main-play svg { width: 26px; height: 26px; fill: #fff; }

        .volume-box { flex: 1; display: flex; flex-direction: column; gap: 8px; }
        
        .volume-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .volume-icon-wrapper {
            display: flex;
            align-items: center;
            gap: 6px;
            color: var(--muted);
        }

        .volume-icon-wrapper svg { width: 14px; height: 14px; fill: currentColor; }
        .volume-label { font-size: 10px; font-weight: 800; text-transform: uppercase; letter-spacing: 0.05em; }
        
        #volPerc { font-size: 10px; font-weight: 800; color: var(--pk); opacity: 0.9; }

        .volume-slider-row {
            display: flex;
            align-items: center;
            gap: 12px;
            width: 100%;
        }

        input[type="range"] {
            flex: 1;
            -webkit-appearance: none;
            background: #333;
            height: 4px;
            border-radius: 10px;
            outline: none;
        }
        input[type="range"]::-webkit-slider-thumb {
            -webkit-appearance: none;
            width: 16px;
            height: 16px;
            background: #fff;
            border-radius: 50%;
            cursor: pointer;
            box-shadow: 0 0 10px rgba(0,0,0,0.5);
            border: 2px solid var(--pk);
        }

        #muteBtn {
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            color: var(--muted);
            transition: color 0.2s, transform 0.2s;
            flex-shrink: 0;
        }
        #muteBtn:hover { color: var(--pk); transform: scale(1.1); }
        #muteBtn svg { width: 18px; height: 18px; fill: currentColor; }

        @keyframes pulse {
            0% { transform: scale(1); opacity: 1; }
            50% { transform: scale(1.4); opacity: 0.4; }
            100% { transform: scale(1); opacity: 1; }
        }
    </style>
</head>
<body>

<div id="dynbg"></div>

<div class="shell">
    <div class="top-bar">
        <img src="https://i.imgur.com/dd4YhU2.png" alt="Radio Selkit Logo" class="brand-logo">
    </div>

    <div class="glass-card">
        <div id="historyPanel">
            <div class="history-header">
                <span class="history-title">HISTORIAL</span>
                <span class="close-history" id="closeHistory">CERRAR</span>
            </div>
            <div id="historyList"></div>
        </div>

        <div class="header-meta">
            <div class="header-left">
                <div class="dj-avatar-container" id="djAvatar">
                    <img id="djImg" src="" alt="DJ">
                    <span id="djInitial">AD</span>
                </div>
                <div class="dj-info-mini">
                    <div class="dj-label">
                        <span id="livePulse" style="width:6px; height:6px; background:var(--pk); border-radius:50%; display:none; animation: pulse 1.5s infinite;"></span>
                        <span id="djStatus">MODO AUTO</span>
                    </div>
                    <div id="djName" class="dj-name-txt">Cargando...</div>
                </div>
            </div>

            <div class="header-right">
                <div class="card-clock">
                    <div id="clockTime">00:00</div>
                    <div id="clockAmPm">PM</div>
                </div>
                <div class="listeners-mini">
                    OYENTES: <span id="listenersCount">0</span>
                </div>
                <div class="history-trigger" id="openHistory">
                    <svg viewBox="0 0 24 24"><path d="M13 3c-4.97 0-9 4.03-9 9H1l3.89 3.89.07.14L9 12H6c0-3.87 3.13-7 7-7s7 3.13 7 7-3.13 7-7 7c-1.93 0-3.68-.79-4.94-2.06l-1.42 1.42C8.27 19.99 10.51 21 13 21c4.97 0 9-4.03 9-9s-4.03-9-9-9zm-1 5v5l4.28 2.54.72-1.21-3.5-2.08V8H12z"/></svg>
                </div>
            </div>
        </div>

        <div class="vinyl-area">
            <div class="vinyl-ring" id="vinyl">
                <img id="cover" class="vinyl-img" src="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='180' height='180'%3E%3Crect width='100%25' height='100%25' fill='%23111'/%3E%3C/svg%3E">
            </div>
        </div>

        <div class="track-info">
            <div class="marquee">
                <span class="marquee-inner" id="trackTitle">Sincronizando señal...</span>
            </div>
            <div id="trackArtist" class="artist-name">RADIO SELKIT</div>
        </div>

        <div class="player-controls">
            <button class="main-play" id="playBtn">
                <svg id="playIcon" viewBox="0 0 24 24"><path d="M8 5v14l11-7z"/></svg>
            </button>
            <div class="volume-box">
                <div class="volume-header">
                    <div class="volume-icon-wrapper">
                        <svg viewBox="0 0 24 24"><path d="M3 9v6h4l5 5V4L7 9H3zm13.5 3c0-1.77-1.02-3.29-2.5-4.03v8.05c1.48-.73 2.5-2.25 2.5-4.02zM14 3.23v2.06c2.89.86 5 3.54 5 6.71s-2.11 5.85-5 6.71v2.06c4.01-.91 7-4.49 7-8.77s-2.99-7.86-7-8.77z"/></svg>
                        <span class="volume-label">VOLUMEN</span>
                    </div>
                    <span id="volPerc">80%</span>
                </div>
                <div class="volume-slider-row">
                    <input type="range" id="volRange" min="0" max="1" step="0.01" value="0.8">
                    <div id="muteBtn" title="Silenciar">
                        <svg id="muteIcon" viewBox="0 0 24 24"><path d="M3 9v6h4l5 5V4L7 9H3zm13.5 3c0-1.77-1.02-3.29-2.5-4.03v8.05c1.48-.73 2.5-2.25 2.5-4.02z"/></svg>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>

<script>
    const CONFIG = {
        stream: "https://radiostreaming.pro:7144/;stream.mp3",
        stats: "https://radiostreaming.pro/cp/get_info.php?p=8290",
        poll: 7000
    };

    let audio = new Audio();
    audio.volume = 0.8;
    let lastVolume = 0.8;
    let isPlaying = false;

    const $ = id => document.getElementById(id);
    const ui = {
        playBtn: $('playBtn'),
        playIcon: $('playIcon'),
        trackTitle: $('trackTitle'),
        trackArtist: $('trackArtist'),
        cover: $('cover'),
        bg: $('dynbg'),
        listeners: $('listenersCount'),
        djName: $('djName'),
        djAvatar: $('djAvatar'),
        djImg: $('djImg'),
        djInitial: $('djInitial'),
        djStatus: $('djStatus'),
        livePulse: $('livePulse'),
        vinyl: $('vinyl'),
        historyPanel: $('historyPanel'),
        historyList: $('historyList'),
        muteBtn: $('muteBtn'),
        muteIcon: $('muteIcon'),
        volRange: $('volRange'),
        volPerc: $('volPerc')
    };

    $('openHistory').onclick = () => ui.historyPanel.classList.add('open');
    $('closeHistory').onclick = () => ui.historyPanel.classList.remove('open');

    function updateClock() {
        const now = new Date();
        const hours = String(now.getHours()).padStart(2, '0');
        const minutes = String(now.getMinutes()).padStart(2, '0');
        const ampm = now.getHours() >= 12 ? 'PM' : 'AM';
        $('clockTime').textContent = `${hours}:${minutes}`;
        $('clockAmPm').textContent = ampm;
    }

    async function fetchStats() {
        try {
            const r = await fetch(`${CONFIG.stats}&t=${Date.now()}`);
            const data = await r.json();
            ui.listeners.textContent = data.listeners || 0;
            const song = data.title || "";
            if (ui.trackTitle.textContent !== song && song !== "") {
                updateTrackUI(song, data.art);
            }
            checkDJ(data);
            if (data.history) updateHistory(data.history);
        } catch (e) {}
    }

    function updateHistory(history) {
        if (!history) return;
        ui.historyList.innerHTML = '';
        history.slice(0, 10).forEach(item => {
            const parts = item.title.split(/ - | – /);
            const artist = parts[0] || "Selkit";
            const title = parts[1] || item.title;
            const art = item.art && !item.art.includes('no-image') ? item.art : 'data:image/svg+xml,%3Csvg xmlns="http://www.w3.org/2000/svg" width="44" height="44"%3E%3Crect width="100%25" height="100%25" fill="%23222"/%3E%3C/svg%3E';
            const div = document.createElement('div');
            div.className = 'history-item';
            div.innerHTML = `<img src="${art}" class="h-art"><div class="h-info"><div class="h-tit">${title}</div><div class="h-artst">${artist}</div></div>`;
            ui.historyList.appendChild(div);
        });
    }

    function checkDJ(data) {
        let djRaw = data.djusername ? data.djusername.trim().toUpperCase() : "";
        const isLive = (djRaw !== "" && djRaw !== "NONE" && djRaw !== "NO DJ" && !djRaw.includes("AUTODJ"));
        const name = isLive ? djRaw : "AUTODJ";
        ui.djName.textContent = name;
        if (isLive) {
            ui.djAvatar.classList.add('active');
            ui.djStatus.textContent = "EN DIRECTO";
            ui.djStatus.style.color = "var(--pk)";
            ui.livePulse.style.display = "inline-block";
            if (data.djprofile && data.djprofile !== "none") {
                ui.djImg.src = data.djprofile; ui.djImg.style.display = "block"; ui.djInitial.style.display = "none";
            } else {
                ui.djImg.style.display = "none"; ui.djInitial.style.display = "block"; ui.djInitial.textContent = name.substring(0, 2);
            }
        } else {
            ui.djAvatar.classList.remove('active');
            ui.djStatus.textContent = "MODO AUTO";
            ui.djStatus.style.color = "var(--muted)";
            ui.livePulse.style.display = "none";
            ui.djImg.style.display = "none"; ui.djInitial.style.display = "block"; ui.djInitial.textContent = "AD";
        }
    }

    function updateTrackUI(full, apiArt) {
        const p = full.split(/ - | – /);
        const art = p[0] || "Radio Selkit";
        const tit = p[1] || (p[0] !== full ? p[0] : full);
        ui.trackTitle.textContent = tit;
        ui.trackArtist.textContent = art;
        if (apiArt && apiArt !== "" && !apiArt.includes("no-image")) {
            ui.cover.src = apiArt; ui.bg.style.backgroundImage = `url(${apiArt})`;
        } else {
            fetch(`https://itunes.apple.com/search?term=${encodeURIComponent(full)}&limit=1&entity=song`)
                .then(res => res.json())
                .then(data => {
                    const img = data.results?.[0]?.artworkUrl100.replace('100x100', '600x600') || "";
                    if (img) { ui.cover.src = img; ui.bg.style.backgroundImage = `url(${img})`; }
                }).catch(() => {});
        }
    }

    ui.playBtn.onclick = () => {
        if (!isPlaying) {
            audio.src = CONFIG.stream; audio.play().catch(() => {});
            ui.playIcon.innerHTML = '<path d="M6 19h4V5H6v14zm8-14v14h4V5h-4z"/>';
            ui.vinyl.classList.add('spinning');
            isPlaying = true;
        } else {
            audio.pause(); audio.src = "";
            ui.playIcon.innerHTML = '<path d="M8 5v14l11-7z"/>';
            ui.vinyl.classList.remove('spinning');
            isPlaying = false;
        }
    };

    function updateVolumeUI(val) {
        ui.volPerc.textContent = Math.round(val * 100) + "%";
        ui.volRange.value = val;
        if (val == 0) {
            ui.muteIcon.innerHTML = '<path d="M16.5 12c0-1.77-1.02-3.29-2.5-4.03v2.21l2.45 2.45c.03-.2.05-.41.05-.63zm2.5 0c0 .94-.2 1.82-.54 2.64l1.51 1.51C20.63 14.91 21 13.5 21 12c0-4.28-2.99-7.86-7-8.77v2.06c2.89.86 5 3.54 5 6.71zM4.27 3L3 4.27 7.73 9H3v6h4l5 5v-6.73l4.25 4.25c-.67.52-1.42.93-2.25 1.18v2.06c1.38-.31 2.63-.95 3.69-1.81L19.73 21 21 19.73l-9-9L4.27 3zM12 4L9.91 6.09 12 8.18V4z"/>';
        } else {
            ui.muteIcon.innerHTML = '<path d="M3 9v6h4l5 5V4L7 9H3zm13.5 3c0-1.77-1.02-3.29-2.5-4.03v8.05c1.48-.73 2.5-2.25 2.5-4.02z"/>';
        }
    }

    ui.volRange.oninput = (e) => {
        const val = e.target.value;
        audio.volume = val;
        if (val > 0) lastVolume = val;
        updateVolumeUI(val);
    };

    ui.muteBtn.onclick = () => {
        if (audio.volume > 0) {
            lastVolume = audio.volume;
            audio.volume = 0;
            updateVolumeUI(0);
        } else {
            audio.volume = lastVolume || 0.8;
            updateVolumeUI(audio.volume);
        }
    };

    setInterval(updateClock, 1000);
    updateClock();
    setInterval(fetchStats, CONFIG.poll);
    fetchStats();
</script>
</body>
</html>
