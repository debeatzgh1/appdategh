<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>AppDateGH — Stream Hub (Chromecast + AirPlay + Auto Update)</title>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <style>
    :root{ --bg:#071018; --panel:#0f1620; --muted:#93a1b2; --accent:#FF2D55; --glass: rgba(255,255,255,0.03); --card-radius:14px; }
    *{box-sizing:border-box}
    html,body{height:100%; margin:0; font-family:Inter,system-ui,-apple-system,Segoe UI,Roboto,Arial; background:linear-gradient(180deg,#071018,#07141a); color:#e6eef6}
    header{position:sticky; top:0; z-index:60; display:flex; gap:12px; align-items:center; padding:10px 16px; background:rgba(3,6,8,0.45); backdrop-filter:blur(6px); border-bottom:1px solid rgba(255,255,255,0.03)}
    header img{height:44px;border-radius:8px}
    header h1{font-size:1.05rem}
    nav{margin-left:auto; display:flex; gap:10px; align-items:center}
    nav a{color:var(--muted); text-decoration:none; font-weight:600}
    main{display:grid; grid-template-columns: 1fr 360px; gap:18px; padding:20px; max-width:1200px; margin:20px auto; width:calc(100% - 40px)}
    @media (max-width:980px){ main{grid-template-columns: 1fr; padding:12px} .sidebar{order:2} .player-wrap{order:1} }
    .player-wrap{background:var(--panel); border-radius:var(--card-radius); padding:14px; box-shadow:0 8px 30px rgba(0,0,0,0.6); border:1px solid rgba(255,255,255,0.03)}
    .player-frame{width:100%; aspect-ratio:16/9; border-radius:10px; overflow:hidden; background:#000; display:flex; align-items:center; justify-content:center}
    .player-controls{display:flex; gap:10px; align-items:center; margin-top:12px; justify-content:space-between}
    .btn{background:var(--glass); border:1px solid rgba(255,255,255,0.04); padding:8px 12px; border-radius:12px; color:inherit; cursor:pointer; font-weight:700; display:inline-flex; align-items:center; gap:8px}
    .btn.primary{background:linear-gradient(90deg,var(--accent),#ff6b7a); color:white}
    .progress{height:6px; background:rgba(255,255,255,0.04); border-radius:999px; overflow:hidden; margin-top:10px}
    .progress > i{display:block; height:100%; width:0%; background: linear-gradient(90deg,var(--accent),#ff6b7a)}
    .meta{display:flex; flex-direction:column; gap:6px; margin-top:10px}
    .title{font-weight:800; font-size:1.05rem}
    .subtitle{color:var(--muted); font-size:0.85rem}
    .card{background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01)); border-radius:12px; padding:12px; margin-bottom:14px; border:1px solid rgba(255,255,255,0.03)}
    .playlist{display:flex; flex-direction:column; gap:8px; max-height:60vh; overflow:auto; padding-right:6px}
    .track{display:flex; gap:10px; align-items:center; padding:8px; border-radius:10px; cursor:pointer; transition:0.15s; border:1px solid transparent}
    .track:hover{background:rgba(255,255,255,0.02)}
    .track.active{background:linear-gradient(90deg,rgba(255,45,85,0.08), rgba(255,107,122,0.03)); border-color:rgba(255,45,85,0.12)}
    .thumb{width:110px;height:62px;background:#111;border-radius:6px;flex-shrink:0; display:flex; align-items:center; justify-content:center; color:var(--muted); font-weight:700}
    .small{font-size:0.82rem;color:var(--muted)}
    .mini-player{position:fixed; right:18px; bottom:18px; z-index:90; width:320px; border-radius:12px; overflow:hidden; box-shadow:0 30px 50px rgba(0,0,0,0.6); border:1px solid rgba(255,255,255,0.04); display:flex; align-items:center; gap:10px; padding:10px; background:linear-gradient(180deg,#071018,#071218); transform:translateY(0); transition:0.2s}
    .mini-player.hidden{opacity:0; pointer-events:none; transform:translateY(16px)}
    footer{max-width:1200px;margin:20px auto;color:var(--muted); padding:20px; text-align:center; font-size:0.9rem}
    .mode-dock{position:fixed; left:50%; transform:translateX(-50%); bottom:86px; display:flex; gap:8px; z-index:80}
    .dock-dot{width:12px;height:12px;border-radius:50%; cursor:pointer; box-shadow:0 6px 18px rgba(0,0,0,0.5)}
    .dot-accent{background:var(--accent)} .dot-cyan{background:#00f2ff} .dot-green{background:#1DB954}
  </style>
</head>
<body>

  <header>
    <img src="https://debeatzgh.wordpress.com/wp-content/uploads/2026/01/gemini_generated_image_e3b3h0e3b3h0e3b38843226607488610379.png" alt="AppDateGH logo">
    <h1>AppDateGH — E-hub</h1>
    <nav>
      <a href="#player">Player</a>
      <a href="#blog">Blog</a>
      <a href="#social">Social</a>
    </nav>
  </header>

  <main>
    <section class="player-wrap" id="player">
      <div class="player-frame" id="player-frame">
        <div id="player-placeholder" style="width:100%;height:100%;display:flex;align-items:center;justify-content:center;color:var(--muted)">
          Loading player…
        </div>
      </div>

      <div class="progress" aria-hidden="true"><i id="seek-fill"></i></div>

      <div class="player-controls" role="group" aria-label="Player controls">
        <div style="display:flex; gap:8px; align-items:center">
          <button id="playBtn" class="btn primary" aria-pressed="false"><i class="fa fa-play"></i><span class="small">Play</span></button>
          <button id="prevBtn" class="btn"><i class="fa fa-backward"></i></button>
          <button id="nextBtn" class="btn"><i class="fa fa-forward"></i></button>
          <button id="pipBtn" class="btn" title="Mini player"><i class="fa fa-window-minimize"></i></button>
          <button id="castBtn" class="btn" title="Cast (Chromecast)"><i class="fa fa-tv"></i> Cast</button>
          <button id="airplayBtn" class="btn" title="AirPlay / Open in YouTube"><i class="fa fa-wifi"></i> AirPlay</button>
        </div>

        <div style="display:flex; gap:8px; align-items:center">
          <button id="refreshBtn" class="btn" title="Refresh playlists"><i class="fa fa-sync"></i> Refresh</button>
          <button id="openPlaylistBtn" class="btn" title="Open playlist on YouTube"><i class="fa fa-folder-open"></i> Open Playlist</button>
        </div>
      </div>

      <div class="meta">
        <div class="title" id="nowTitle">Loading playlist…</div>
        <div class="subtitle" id="nowChannel">—</div>
      </div>
    </section>

    <aside class="sidebar">
      <div class="card">
        <h3 style="margin:0 0 8px 0">Up Next</h3>
        <div class="playlist" id="playlist" role="list" aria-label="Playlist"></div>
      </div>

      <div class="card" id="blog">
        <h3 style="margin:0 0 8px 0">Latest Blog Posts</h3>
        <div class="posts-list" id="blog-posts">Loading posts…</div>
      </div>

      <div class="card">
        <h3 style="margin:0 0 8px 0">Follow</h3>
        <div style="display:flex;gap:8px">
          <a class="btn" href="https://www.youtube.com/@debeatzgh" target="_blank" rel="noopener"><i class="fa fa-youtube"></i> YouTube</a>
          <a class="btn" href="https://www.instagram.com/debeatzgh" target="_blank" rel="noopener"><i class="fa fa-instagram"></i> Instagram</a>
        </div>
      </div>
    </aside>
  </main>

  <div id="miniPlayer" class="mini-player hidden" aria-hidden="true">
    <div style="width:110px;height:62px;border-radius:8px;background:#000;display:flex;align-items:center;justify-content:center" id="miniThumb">VID</div>
    <div style="flex:1">
      <div id="miniTitle" style="font-weight:800">—</div>
      <div class="small" id="miniChannel">—</div>
    </div>
    <div style="display:flex;gap:8px;align-items:center">
      <button id="miniPrev" class="btn"><i class="fa fa-backward"></i></button>
      <button id="miniPlay" class="btn"><i class="fa fa-play"></i></button>
      <button id="miniNext" class="btn"><i class="fa fa-forward"></i></button>
      <button id="miniClose" class="btn"><i class="fa fa-times"></i></button>
    </div>
  </div>

  <div class="mode-dock" role="toolbar" aria-label="Theme">
    <div class="dock-dot dot-accent" title="Default" onclick="applyAccent()"></div>
    <div class="dock-dot dot-cyan" title="Cyan" onclick="applyCyan()"></div>
    <div class="dock-dot dot-green" title="Spotify" onclick="applyGreen()"></div>
  </div>

  <footer>&copy; 2026 AppDateGH — Shortcuts: Space=Play/Pause, N=Next, P=Prev, M=Mute.</footer>

  <!-- Cast SDK -->
  <script src="https://www.gstatic.com/cv/js/sender/v1/cast_sender.js?loadCastFramework=1"></script>
  <!-- YouTube IFrame API -->
  <script src="https://www.youtube.com/iframe_api"></script>

  <script>
    // Provided sources (no API key)
    const CHANNEL_HANDLE = '@debeatzgh';
    const CHANNEL_URL = 'https://youtube.com/@debeatzgh';
    const PLAYLIST_URL = 'https://www.youtube.com/feeds/videos.xml?playlist_id=PLMOQxjh_hNfRbNjMEMpG_wBsf4NODJGEG';

    // Internal state
    let PLAYLIST = [
      // fallback items — will be replaced by auto-update
      { id: '7nSr2P6blYM', title: 'Featured Video — AppDateGH', channel: 'AppDateGH' }
    ];
    let player, currentIndex = 0, isPlaying = false;
    const playlistEl = document.getElementById('playlist');

    // ----- YouTube Player -----
    function onYouTubeIframeAPIReady() {
      player = new YT.Player('player-frame', {
        height: '100%',
        width: '100%',
        videoId: PLAYLIST[currentIndex].id,
        playerVars: { autoplay: 0, controls: 1, modestbranding: 1, rel: 0, enablejsapi: 1, playsinline: 1 },
        events: { onReady: onPlayerReady, onStateChange: onPlayerStateChange }
      });
    }

    function onPlayerReady() {
      document.getElementById('player-placeholder')?.remove();
      renderPlaylist();
      bindUI();
      updateNow();
      applyPersistedTheme();
      // initial auto updates
      autoUpdateAll();
      setInterval(autoUpdateAll, 1000 * 60 * 3); // refresh every 3 minutes
    }

    function onPlayerStateChange(e) {
      const state = e.data;
      const playBtn = document.getElementById('playBtn');
      const miniPlay = document.getElementById('miniPlay');
      if (state === YT.PlayerState.PLAYING) {
        playBtn.innerHTML = '<i class="fa fa-pause"></i><span class="small">Pause</span>';
        miniPlay.innerHTML = '<i class="fa fa-pause"></i>';
        isPlaying = true; startProgress();
      } else {
        playBtn.innerHTML = '<i class="fa fa-play"></i><span class="small">Play</span>';
        miniPlay.innerHTML = '<i class="fa fa-play"></i>';
        isPlaying = false; stopProgress();
      }
      if (state === YT.PlayerState.ENDED) nextIndex();
    }

    // progress
    let progressTimer = null;
    function startProgress(){
      stopProgress();
      progressTimer = setInterval(()=> {
        if(!player || !player.getDuration) return;
        const d = player.getDuration();
        if(d>0){ const cur = player.getCurrentTime(); const pct = Math.min(100, (cur/d)*100); document.getElementById('seek-fill').style.width = pct + '%'; }
      }, 600);
    }
    function stopProgress(){ if(progressTimer){ clearInterval(progressTimer); progressTimer = null; } }

    // controls
    function togglePlay(){ if(!player) return; const s = player.getPlayerState(); if(s === YT.PlayerState.PLAYING) player.pauseVideo(); else player.playVideo(); }
    function nextIndex(){ currentIndex = (currentIndex + 1) % PLAYLIST.length; loadIndex(currentIndex); }
    function prevIndex(){ currentIndex = (currentIndex - 1 + PLAYLIST.length) % PLAYLIST.length; loadIndex(currentIndex); }
    function loadIndex(i){ currentIndex = i; localStorage.setItem('appdate-current', currentIndex); player.loadVideoById(PLAYLIST[currentIndex].id); updateNow(); renderPlaylist(); }

    function updateNow(){
      const cur = PLAYLIST[currentIndex] || { title: '—', channel: '—', id: '' };
      document.getElementById('nowTitle').textContent = cur.title;
      document.getElementById('nowChannel').textContent = cur.channel;
      document.getElementById('miniTitle').textContent = cur.title;
      document.getElementById('miniChannel').textContent = cur.channel;
      document.getElementById('miniThumb').innerHTML = cur.id ? `<img decoding="async" src="https://i.ytimg.com/vi/${cur.id}/mqdefault.jpg" alt="" style="width:100%;height:100%;object-fit:cover;border-radius:6px">` : 'VID';
    }

    function renderPlaylist(){
      playlistEl.innerHTML = '';
      PLAYLIST.forEach((item, i)=>{
        const el = document.createElement('div');
        el.className = 'track' + (i === currentIndex ? ' active' : '');
        el.tabIndex = 0;
        el.innerHTML = `<div class="thumb" aria-hidden="true"><img decoding="async" src="https://i.ytimg.com/vi/${item.id}/mqdefault.jpg" alt="" style="width:100%;height:100%;object-fit:cover;border-radius:6px"></div>
                        <div style="flex:1"><div style="font-weight:800">${item.title}</div><div class="small">${item.channel}</div></div>`;
        el.addEventListener('click', ()=> loadIndex(i));
        el.addEventListener('keydown', (ev)=> { if(ev.key === 'Enter') loadIndex(i); });
        playlistEl.appendChild(el);
      });
    }

    // Bind UI
    function bindUI(){
      document.getElementById('playBtn').addEventListener('click', togglePlay);
      document.getElementById('prevBtn').addEventListener('click', prevIndex);
      document.getElementById('nextBtn').addEventListener('click', nextIndex);
      document.getElementById('pipBtn').addEventListener('click', toggleMiniMode);
      document.getElementById('refreshBtn').addEventListener('click', autoUpdateAll);
      document.getElementById('openPlaylistBtn').addEventListener('click', ()=> window.open('https://www.youtube.com/playlist?list=PLMOQxjh_hNfRbNjMEMpG_wBsf4NODJGEG','_blank'));
      document.getElementById('miniPlay').addEventListener('click', togglePlay);
      document.getElementById('miniPrev').addEventListener('click', prevIndex);
      document.getElementById('miniNext').addEventListener('click', nextIndex);
      document.getElementById('miniClose').addEventListener('click', ()=> hideMiniMode());
      document.getElementById('castBtn').addEventListener('click', startCastFlow);
      document.getElementById('airplayBtn').addEventListener('click', airPlayOrOpen);

      // keyboard shortcuts
      window.addEventListener('keydown', (e)=>{
        if(['INPUT','TEXTAREA'].includes(document.activeElement.tagName)) return;
        if(e.code === 'Space'){ e.preventDefault(); togglePlay(); }
        if(e.key === 'n' || e.key === 'N') nextIndex();
        if(e.key === 'p' || e.key === 'P') prevIndex();
        if(e.key === 'm' || e.key === 'M') { if(player) { if(player.isMuted()) { player.unMute(); } else { player.mute(); } } }
        if(e.key === 'f' || e.key === 'F') { const iframe = player && player.getIframe ? player.getIframe() : null; if(iframe) { if(document.fullscreenElement) document.exitFullscreen(); else iframe.requestFullscreen().catch(()=>{}); } }
      });

      // restore saved index
      const saved = localStorage.getItem('appdate-current');
      if(saved) { currentIndex = parseInt(saved,10) % PLAYLIST.length; }
    }

    // Mini
    function toggleMiniMode(){ const mini = document.getElementById('miniPlayer'); if(mini.classList.contains('hidden')) showMiniMode(); else hideMiniMode(); }
    function showMiniMode(){ document.getElementById('miniPlayer').classList.remove('hidden'); document.getElementById('miniPlayer').setAttribute('aria-hidden','false'); }
    function hideMiniMode(){ document.getElementById('miniPlayer').classList.add('hidden'); document.getElementById('miniPlayer').setAttribute('aria-hidden','true'); }

    // ----- Auto-update logic (no API key) -----
    // Fetch playlist RSS (works without API key)
    async function fetchPlaylistRss(url){
      try {
        const res = await fetch(url);
        if(!res.ok) throw new Error('Playlist RSS fetch failed');
        const text = await res.text();
        const parser = new DOMParser();
        const xml = parser.parseFromString(text, 'application/xml');
        const entries = Array.from(xml.querySelectorAll('entry'));
        return entries.map(e=>{
          const vid = e.querySelector('yt\\:videoId')?.textContent || '';
          const title = e.querySelector('title')?.textContent || 'Untitled';
          const author = e.querySelector('author > name')?.textContent || '';
          return { id: vid, title, channel: author };
        });
      } catch (err) {
        console.warn('Playlist RSS error', err);
        return [];
      }
    }

    // Try to discover the channelId using oEmbed (best-effort). If CORS blocks, fallback to attempt "user" feed from handle.
    async function discoverChannelIdFromHandle(handleOrUrl){
      try {
        // try oEmbed first
        const oembedUrl = `https://www.youtube.com/oembed?url=${encodeURIComponent('https://www.youtube.com/' + handleOrUrl)}&format=json`;
        const r = await fetch(oembedUrl);
        if(r.ok){
          const j = await r.json();
          // author_url often contains /channel/UCxxx
          if(j.author_url && j.author_url.includes('/channel/')){
            const parts = j.author_url.split('/');
            const channelId = parts[parts.length - 1];
            console.log('Discovered channelId via oEmbed', channelId);
            return channelId;
          }
        } else {
          console.warn('oEmbed blocked or not OK', r.status);
        }
      } catch (err) {
        console.warn('oEmbed error', err);
      }

      // fallback: attempt user feed using handle without '@'
      try {
        const maybeUser = handleOrUrl.replace(/^@/, '').replace(/^\/+/, '');
        const userFeed = `https://www.youtube.com/feeds/videos.xml?user=${encodeURIComponent(maybeUser)}`;
        const res = await fetch(userFeed);
        if(res.ok){
          // If it returns XML, we can parse and also capture channel id from feed root's <author><uri> maybe
          const text = await res.text();
          const parser = new DOMParser();
          const xml = parser.parseFromString(text, 'application/xml');
          const channelIdNode = xml.querySelector('feed > author > uri') || xml.querySelector('feed > author > email');
          // channelId might not be in those nodes; better to extract from any link: find link rel="alternate" pointing to channel
          const link = Array.from(xml.querySelectorAll('link')).find(l=>l.getAttribute('rel')==='alternate');
          if(link && link.getAttribute('href')){
            const href = link.getAttribute('href');
            const m = href.match(/\/channel\/(UC[0-9A-Za-z_-]{22})/);
            if(m) return m[1];
          }
         
