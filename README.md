
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>AppDateGH — Stream Hub</title>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <style>
    :root{ --bg:#071018; --panel:#0f1620; --muted:#93a1b2; --accent:#FF2D55; --glass: rgba(255,255,255,0.03); --card-radius:14px; }
    *{box-sizing:border-box}
    html,body{height:100%; margin:0; font-family:Inter,system-ui,-apple-system; background:linear-gradient(180deg,#071018,#07141a); color:#e6eef6; overflow-x: hidden;}
    header{position:sticky; top:0; z-index:60; display:flex; gap:12px; align-items:center; padding:10px 16px; background:rgba(3,6,8,0.7); backdrop-filter:blur(12px); border-bottom:1px solid rgba(255,255,255,0.05)}
    header img{height:40px; border-radius:8px}
    header h1{font-size:1rem; font-weight: 800; letter-spacing: -0.5px;}
    nav{margin-left:auto; display:flex; gap:15px; align-items:center}
    nav a{color:var(--muted); text-decoration:none; font-weight:600; font-size: 0.85rem; transition: 0.2s;}
    nav a:hover{color:var(--accent)}
    main{display:grid; grid-template-columns: 1fr 360px; gap:18px; padding:20px; max-width:1200px; margin:0 auto; width:100%}
    @media (max-width:980px){ main{grid-template-columns: 1fr; padding:12px} .sidebar{order:2} .player-wrap{order:1} }
    .player-wrap{background:var(--panel); border-radius:var(--card-radius); padding:14px; box-shadow:0 8px 30px rgba(0,0,0,0.6); border:1px solid rgba(255,255,255,0.03)}
    .player-frame{width:100%; aspect-ratio:16/9; border-radius:10px; overflow:hidden; background:#000; display:flex; align-items:center; justify-content:center}
    .player-controls{display:flex; gap:10px; align-items:center; margin-top:12px; justify-content:space-between; flex-wrap: wrap;}
    .btn{background:var(--glass); border:1px solid rgba(255,255,255,0.06); padding:8px 12px; border-radius:12px; color:inherit; cursor:pointer; font-weight:700; display:inline-flex; align-items:center; gap:8px; transition: 0.2s;}
    .btn:hover{background: rgba(255,255,255,0.1); border-color: rgba(255,255,255,0.2);}
    .btn.primary{background:linear-gradient(90deg,var(--accent),#ff6b7a); color:white; border:none;}
    .progress{height:4px; background:rgba(255,255,255,0.04); border-radius:999px; overflow:hidden; margin-top:15px}
    .progress > i{display:block; height:100%; width:0%; background: var(--accent); transition: width 0.3s ease;}
    .meta{margin-top:15px}
    .title{font-weight:800; font-size:1.1rem; margin-bottom: 4px}
    .subtitle{color:var(--muted); font-size:0.85rem}
    .card{background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01)); border-radius:12px; padding:15px; margin-bottom:14px; border:1px solid rgba(255,255,255,0.03)}
    .playlist{display:flex; flex-direction:column; gap:8px; max-height:450px; overflow-y:auto; padding-right:5px}
    .playlist::-webkit-scrollbar { width: 4px; }
    .playlist::-webkit-scrollbar-thumb { background: rgba(255,255,255,0.1); border-radius: 10px; }
    .track{display:flex; gap:12px; align-items:center; padding:10px; border-radius:10px; cursor:pointer; transition:0.15s; border:1px solid transparent}
    .track:hover{background:rgba(255,255,255,0.03)}
    .track.active{background:rgba(255,45,85,0.1); border-color:rgba(255,45,85,0.2)}
    .thumb{width:100px; height:56px; background:#111; border-radius:6px; flex-shrink:0; overflow:hidden;}
    .thumb img { width: 100%; height: 100%; object-fit: cover; }
    .mini-player{position:fixed; right:18px; bottom:18px; z-index:100; width:340px; border-radius:16px; overflow:hidden; box-shadow:0 20px 40px rgba(0,0,0,0.8); border:1px solid rgba(255,255,255,0.08); display:flex; align-items:center; gap:12px; padding:12px; background:#0f1620; transition:0.3s cubic-bezier(0.4, 0, 0.2, 1)}
    .mini-player.hidden{opacity:0; pointer-events:none; transform:translateY(30px) scale(0.95)}
    .mode-dock{position:fixed; left:50%; transform:translateX(-50%); bottom:20px; display:flex; gap:12px; z-index:80; background: rgba(0,0,0,0.5); padding: 8px 16px; border-radius: 100px; backdrop-filter: blur(10px); border: 1px solid rgba(255,255,255,0.1);}
    .dock-dot{width:16px; height:16px; border-radius:50%; cursor:pointer; transition: 0.2s;}
    .dock-dot:hover { transform: scale(1.2); }
    .dot-accent{background:var(--accent)} .dot-cyan{background:#00f2ff} .dot-green{background:#1DB954}
  </style>
</head>
<body>

  <header>
    <img src="https://debeatzgh.wordpress.com/wp-content/uploads/2026/01/gemini_generated_image_e3b3h0e3b3h0e3b38843226607488610379.png" alt="AppDateGH logo">
    <h1>AppDateGH — E-hub</h1>
    <nav>
      <a href="#player">Player</a>
      <a href="https://beatzde4.blogspot.com" target="_blank">Blog</a>
      <a href="https://www.youtube.com/@debeatzgh" target="_blank">YouTube</a>
    </nav>
  </header>

  <main>
    <section class="player-wrap" id="player">
      <div class="player-frame" id="player-frame">
        <div id="player-placeholder" style="color:var(--muted)">Initializing Stream Hub...</div>
      </div>

      <div class="progress" aria-hidden="true"><i id="seek-fill"></i></div>

      <div class="player-controls">
        <div style="display:flex; gap:8px; align-items:center">
          <button id="playBtn" class="btn primary"><i class="fa fa-play"></i></button>
          <button id="prevBtn" class="btn"><i class="fa fa-backward"></i></button>
          <button id="nextBtn" class="btn"><i class="fa fa-forward"></i></button>
          <button id="pipBtn" class="btn" title="Mini player"><i class="fa fa-expand"></i></button>
        </div>

        <div style="display:flex; gap:8px; align-items:center">
          <button id="refreshBtn" class="btn"><i class="fa fa-sync"></i> Refresh</button>
        </div>
      </div>

      <div class="meta">
        <div class="title" id="nowTitle">Loading content...</div>
        <div class="subtitle" id="nowChannel">Please wait</div>
      </div>
    </section>

    <aside class="sidebar">
      <div class="card">
        <h3 style="margin:0 0 12px 0; font-size: 0.9rem; text-transform: uppercase; color: var(--accent);">Queue</h3>
        <div class="playlist" id="playlist"></div>
      </div>

      <div class="card">
        <h3 style="margin:0 0 12px 0; font-size: 0.9rem; text-transform: uppercase;">Connect</h3>
        <div style="display:flex;gap:8px; flex-wrap: wrap;">
          <a class="btn" style="flex:1" href="https://www.youtube.com/@debeatzgh" target="_blank"><i class="fa-brands fa-youtube"></i></a>
          <a class="btn" style="flex:1" href="https://www.instagram.com/debeatzgh" target="_blank"><i class="fa-brands fa-instagram"></i></a>
        </div>
      </div>
    </aside>
  </main>

  <div id="miniPlayer" class="mini-player hidden">
    <div class="thumb" id="miniThumb"></div>
    <div style="flex:1; overflow: hidden;">
      <div id="miniTitle" style="font-weight:800; font-size: 0.8rem; white-space: nowrap; overflow: hidden; text-overflow: ellipsis;">—</div>
      <div class="small" id="miniChannel" style="font-size: 0.7rem;">—</div>
    </div>
    <button id="miniClose" class="btn" style="padding: 5px;"><i class="fa fa-times"></i></button>
  </div>

  <div class="mode-dock">
    <div class="dock-dot dot-accent" onclick="setTheme('default')"></div>
    <div class="dock-dot dot-cyan" onclick="setTheme('cyan')"></div>
    <div class="dock-dot dot-green" onclick="setTheme('green')"></div>
  </div>

  <script src="https://www.youtube.com/iframe_api"></script>

  <script>
    // GitHub Pages Compatible RSS Fetch with Proxy
    const PROXY = "https://corsproxy.io/?";
    const RSS_URL = "https://www.youtube.com/feeds/videos.xml?playlist_id=PLMOQxjh_hNfRbNjMEMpG_wBsf4NODJGEG";
    
    let PLAYLIST = [{ id: '7nSr2P6blYM', title: 'Loading...', channel: 'AppDateGH' }];
    let player, currentIndex = 0, isPlaying = false;

    async function fetchVideos() {
        try {
            const response = await fetch(PROXY + encodeURIComponent(RSS_URL));
            const str = await response.text();
            const data = new window.DOMParser().parseFromString(str, "text/xml");
            const entries = Array.from(data.querySelectorAll("entry"));
            
            if (entries.length > 0) {
                PLAYLIST = entries.map(e => ({
                    id: e.querySelector("videoId").textContent,
                    title: e.querySelector("title").textContent,
                    channel: e.querySelector("name").textContent
                }));
                renderPlaylist();
                updateNow();
            }
        } catch (e) {
            console.error("RSS Load Error:", e);
        }
    }

    function onYouTubeIframeAPIReady() {
        player = new YT.Player('player-frame', {
            height: '100%', width: '100%',
            videoId: PLAYLIST[currentIndex].id,
            playerVars: { 'autoplay': 0, 'rel': 0, 'modestbranding': 1, 'playsinline': 1 },
            events: { 
                'onReady': () => { fetchVideos(); bindUI(); },
                'onStateChange': onPlayerStateChange 
            }
        });
    }

    function onPlayerStateChange(e) {
        isPlaying = (e.data === YT.PlayerState.PLAYING);
        const playBtn = document.getElementById('playBtn');
        playBtn.innerHTML = isPlaying ? '<i class="fa fa-pause"></i>' : '<i class="fa fa-play"></i>';
        if (e.data === YT.PlayerState.PLAYING) startProgress();
        else stopProgress();
        if (e.data === YT.PlayerState.ENDED) nextVideo();
    }

    function renderPlaylist() {
        const container = document.getElementById('playlist');
        container.innerHTML = PLAYLIST.map((item, i) => `
            <div class="track ${i === currentIndex ? 'active' : ''}" onclick="loadVideo(${i})">
                <div class="thumb"><img src="https://i.ytimg.com/vi/${item.id}/mqdefault.jpg"></div>
                <div style="flex:1; overflow:hidden">
                    <div style="font-weight:700; font-size:0.85rem; white-space:nowrap; overflow:hidden; text-overflow:ellipsis;">${item.title}</div>
                    <div class="small">${item.channel}</div>
                </div>
            </div>
        `).join('');
    }

    function loadVideo(i) {
        currentIndex = i;
        player.loadVideoById(PLAYLIST[i].id);
        updateNow();
        renderPlaylist();
    }

    function nextVideo() { currentIndex = (currentIndex + 1) % PLAYLIST.length; loadVideo(currentIndex); }
    function prevVideo() { currentIndex = (currentIndex - 1 + PLAYLIST.length) % PLAYLIST.length; loadVideo(currentIndex); }

    function updateNow() {
        const cur = PLAYLIST[currentIndex];
        document.getElementById('nowTitle').innerText = cur.title;
        document.getElementById('nowChannel').innerText = cur.channel;
        document.getElementById('miniTitle').innerText = cur.title;
        document.getElementById('miniChannel').innerText = cur.channel;
        document.getElementById('miniThumb').innerHTML = `<img src="https://i.ytimg.com/vi/${cur.id}/mqdefault.jpg">`;
    }

    let progressInterval;
    function startProgress() {
        progressInterval = setInterval(() => {
            const pct = (player.getCurrentTime() / player.getDuration()) * 100;
            document.getElementById('seek-fill').style.width = pct + '%';
        }, 1000);
    }
    function stopProgress() { clearInterval(progressInterval); }

    function bindUI() {
        document.getElementById('playBtn').onclick = () => isPlaying ? player.pauseVideo() : player.playVideo();
        document.getElementById('nextBtn').onclick = nextVideo;
        document.getElementById('prevBtn').onclick = prevVideo;
        document.getElementById('refreshBtn').onclick = fetchVideos;
        document.getElementById('pipBtn').onclick = () => document.getElementById('miniPlayer').classList.toggle('hidden');
        document.getElementById('miniClose').onclick = () => document.getElementById('miniPlayer').classList.add('hidden');
    }

    function setTheme(t) {
        const root = document.documentElement;
        if(t === 'cyan') root.style.setProperty('--accent', '#00f2ff');
        else if(t === 'green') root.style.setProperty('--accent', '#1DB954');
        else root.style.setProperty('--accent', '#FF2D55');
    }
  </script>
</body>
</html>
