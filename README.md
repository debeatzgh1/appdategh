

<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>AppDateGH — Stream Hub</title>

  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

  <style>
    :root{
      --bg:#0b0f13; --panel:#0f1620; --muted:#93a1b2;
      --accent:#FF2D55; --glass: rgba(255,255,255,0.03);
      --card-radius:14px;
    }
    *{box-sizing:border-box}
    html,body{height:100%; margin:0; font-family:Inter,system-ui,-apple-system,Segoe UI,Roboto,"Helvetica Neue",Arial;}
    body{background:linear-gradient(180deg,#071018 0%, #07141a 60%); color:#e6eef6; -webkit-font-smoothing:antialiased; -moz-osx-font-smoothing:grayscale; line-height:1.35;}
    header{position:sticky; top:0; z-index:60; backdrop-filter: blur(6px); background: linear-gradient(180deg, rgba(3,6,8,0.6), rgba(3,6,8,0.35)); border-bottom:1px solid rgba(255,255,255,0.03); display:flex; align-items:center; gap:16px; padding:10px 16px;}
    header img{height:44px; width:auto; border-radius:8px}
    header h1{font-size:1.05rem; letter-spacing:0.6px}
    nav{margin-left:auto; display:flex; gap:10px; align-items:center}
    nav a{color:var(--muted); text-decoration:none; font-weight:600; font-size:0.9rem}
    main{display:grid; grid-template-columns: 1fr 360px; gap:18px; padding:20px; max-width:1200px; margin:20px auto; width:calc(100% - 40px)}
    @media (max-width:980px){ main{grid-template-columns: 1fr; padding:12px; width:calc(100% - 24px)} .sidebar{order:2} .player-wrap{order:1}}
    .player-wrap{background:var(--panel); border-radius:var(--card-radius); padding:14px; box-shadow: 0 8px 30px rgba(0,0,0,0.6); border:1px solid rgba(255,255,255,0.03)}
    .player-frame{width:100%; aspect-ratio:16/9; border-radius:10px; overflow:hidden; background:#000; display:flex; align-items:center; justify-content:center}
    .player-controls{display:flex; gap:10px; align-items:center; margin-top:12px; justify-content:space-between}
    .controls-left{display:flex; gap:8px; align-items:center}
    .btn{background:var(--glass); border:1px solid rgba(255,255,255,0.04); padding:8px 12px; border-radius:12px; color:inherit; cursor:pointer; font-weight:700; display:inline-flex; align-items:center; gap:8px}
    .btn.primary{background:linear-gradient(90deg,var(--accent),#ff6b7a); color:white; box-shadow:0 8px 30px rgba(255,45,85,0.08)}
    .btn.icon{padding:8px; border-radius:10px}
    .progress{height:6px; background:rgba(255,255,255,0.04); border-radius:999px; overflow:hidden; margin-top:10px}
    .progress > i{display:block; height:100%; width:0%; background: linear-gradient(90deg,var(--accent),#ff6b7a)}
    .meta{display:flex; flex-direction:column; gap:6px; margin-top:10px}
    .title{font-weight:800; font-size:1.05rem}
    .subtitle{color:var(--muted); font-size:0.85rem}

    /* Sidebar */
    .sidebar{background:transparent}
    .card{background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01)); border-radius:12px; padding:12px; margin-bottom:14px; border:1px solid rgba(255,255,255,0.03)}
    .playlist{display:flex; flex-direction:column; gap:8px; max-height:60vh; overflow:auto; padding-right:6px}
    .track{display:flex; gap:10px; align-items:center; padding:8px; border-radius:10px; cursor:pointer; transition:0.15s; border:1px solid transparent}
    .track:hover{background:rgba(255,255,255,0.02)}
    .track.active{background:linear-gradient(90deg,rgba(255,45,85,0.08), rgba(255,107,122,0.03)); border-color:rgba(255,45,85,0.12)}
    .thumb{width:110px;height:62px;background:#111;border-radius:6px;flex-shrink:0; display:flex; align-items:center; justify-content:center; color:var(--muted); font-weight:700}
    .track .meta{margin:0}
    .small{font-size:0.82rem;color:var(--muted)}

    /* Floating mini player */
    .mini-player{position:fixed; right:18px; bottom:18px; z-index:90; width:320px; border-radius:12px; overflow:hidden; box-shadow:0 30px 50px rgba(0,0,0,0.6); border:1px solid rgba(255,255,255,0.04); display:flex; align-items:center; gap:10px; padding:10px; background:linear-gradient(180deg,#071018, #071218); transform:translateY(0); transition:0.2s}
    .mini-player.hidden{opacity:0; pointer-events:none; transform:translateY(16px)}
    .mini-thumb{width:110px;height:62px;border-radius:8px;background:#000; display:flex;align-items:center;justify-content:center}
    .mini-controls{display:flex; gap:8px; align-items:center}
    .pill{padding:6px 10px;border-radius:999px;background:rgba(255,255,255,0.03);font-weight:700}

    /* Blog list */
    .posts-list{display:flex; flex-direction:column; gap:10px}
    .post-item{display:flex; gap:10px; align-items:center; padding:8px; border-radius:10px}
    .post-thumb{width:80px;height:56px;background:#091018;border-radius:8px; flex-shrink:0}
    .post-meta{display:flex; flex-direction:column; gap:4px}
    .socials{display:flex; gap:10px; align-items:center}

    footer{max-width:1200px;margin:20px auto;color:var(--muted); padding:20px; text-align:center; font-size:0.9rem}
    .mode-dock{position:fixed; left:50%; transform:translateX(-50%); bottom:86px; display:flex; gap:8px; z-index:80}
    .dock-dot{width:12px;height:12px;border-radius:50%; cursor:pointer; box-shadow:0 6px 18px rgba(0,0,0,0.5)}
    .dot-accent{background:var(--accent)} .dot-cyan{background:#00f2ff} .dot-green{background:#1DB954}
    /* accessibility focus */
    .btn:focus, .track:focus { outline:2px solid rgba(255,255,255,0.06); outline-offset:3px; }

    /* scrollbars */
    ::-webkit-scrollbar{height:10px;width:8px}
    ::-webkit-scrollbar-thumb{background:rgba(255,255,255,0.04); border-radius:10px}
  </style>
</head>
<body>

  <header role="banner" aria-label="AppDateGH header">
    <img src="https://debeatzgh.wordpress.com/wp-content/uploads/2026/01/gemini_generated_image_e3b3h0e3b3h0e3b38843226607488610379.png" alt="AppDateGH logo">
    <h1>AppDateGH — E-hub</h1>
    <nav role="navigation" aria-label="Primary">
      <a href="#player">Player</a>
      <a href="#blog">Blog</a>
      <a href="#social">Social</a>
    </nav>
  </header>

  <main>
    <section class="player-wrap" id="player" aria-labelledby="player-title">
      <div class="player-frame" id="player-frame" aria-live="polite">
        <!-- YouTube IFrame is injected by the API -->
        <div id="player-placeholder" style="width:100%;height:100%;display:flex;align-items:center;justify-content:center;color:var(--muted)">
          Loading player…
        </div>
      </div>

      <div class="progress" aria-hidden="true"><i id="seek-fill"></i></div>

      <div class="player-controls" role="group" aria-label="Player controls">
        <div class="controls-left">
          <button id="playBtn" class="btn primary" aria-pressed="false"><i class="fa fa-play"></i><span class="small">Play</span></button>
          <button id="prevBtn" class="btn icon" title="Previous (P)"><i class="fa fa-backward"></i></button>
          <button id="nextBtn" class="btn icon" title="Next (N)"><i class="fa fa-forward"></i></button>
          <button id="pipBtn" class="btn icon" title="Toggle Mini Player"><i class="fa fa-window-minimize"></i></button>
          <button id="fsBtn" class="btn icon" title="Fullscreen"><i class="fa fa-expand"></i></button>
        </div>

        <div style="display:flex;gap:8px;align-items:center">
          <select id="qualitySelect" class="btn" aria-label="Playback quality">
            <option value="">Auto</option>
            <option value="highres">1080p</option>
            <option value="hd720">720p</option>
            <option value="large">480p</option>
            <option value="medium">360p</option>
            <option value="small">240p</option>
          </select>
          <button id="muteBtn" class="btn icon" title="Mute (M)"><i class="fa fa-volume-up"></i></button>
          <button id="sleepBtn" class="btn" title="Sleep timer">Sleep Off</button>
        </div>
      </div>

      <div class="meta" aria-hidden="false">
        <div class="title" id="nowTitle">Loading playlist…</div>
        <div class="subtitle" id="nowChannel">—</div>
      </div>
    </section>

    <aside class="sidebar" aria-labelledby="sidebar-title">
      <div class="card">
        <h3 id="sidebar-title" style="margin:0 0 8px 0">Up Next</h3>
        <div class="playlist" id="playlist" role="list" aria-label="Playlist"></div>
      </div>

      <div class="card" id="blog" style="max-height:36vh;overflow:auto;">
        <h3 style="margin:0 0 8px 0">Latest Blog Posts</h3>
        <div class="posts-list" id="blog-posts">Loading posts…</div>
      </div>

      <div class="card">
        <h3 style="margin:0 0 8px 0">Follow</h3>
        <div class="socials">
          <a class="btn" href="https://www.youtube.com/@debeatzgh" target="_blank" rel="noopener"><i class="fa fa-youtube"></i> YouTube</a>
          <a class="btn" href="https://www.instagram.com/debeatzgh" target="_blank" rel="noopener"><i class="fa fa-instagram"></i> Instagram</a>
          <a class="btn" href="https://www.facebook.com/beatzde4" target="_blank" rel="noopener"><i class="fa fa-facebook"></i> Facebook</a>
        </div>
      </div>
    </aside>
  </main>

  <!-- mini player -->
  <div id="miniPlayer" class="mini-player hidden" aria-hidden="true" role="dialog" aria-label="Mini player">
    <div class="mini-thumb" id="miniThumb">VID</div>
    <div style="flex:1">
      <div id="miniTitle" style="font-weight:800;font-size:0.95rem">—</div>
      <div class="small" id="miniChannel">—</div>
    </div>
    <div class="mini-controls">
      <button id="miniPrev" class="btn icon"><i class="fa fa-backward"></i></button>
      <button id="miniPlay" class="btn icon"><i class="fa fa-play"></i></button>
      <button id="miniNext" class="btn icon"><i class="fa fa-forward"></i></button>
      <button id="miniClose" class="btn icon" title="Close"><i class="fa fa-times"></i></button>
    </div>
  </div>

  <div class="mode-dock" role="toolbar" aria-label="Theme modes">
    <div class="dock-dot dot-accent" title="Default" onclick="applyAccent()"></div>
    <div class="dock-dot dot-cyan" title="Cyan" onclick="applyCyan()"></div>
    <div class="dock-dot dot-green" title="Spotify" onclick="applyGreen()"></div>
  </div>

  <footer>&copy; 2026 AppDateGH — Stream-focused experience. Shortcuts: Space=Play/Pause, N=Next, P=Prev, M=Mute.</footer>

  <!-- YouTube IFrame API -->
  <script src="https://www.youtube.com/iframe_api"></script>

  <script>
    // Playlist config: add YouTube IDs and titles
    const PLAYLIST = [
      { id: '7nSr2P6blYM', title: 'Featured Video — AppDateGH', channel: 'AppDateGH' },
      { id: 'r1Fx0tqK5Z4', title: 'Video 2 — Beats & Tips', channel: 'DebeatzGH' },
      { id: 'r2H51YxxEns', title: 'Video 3 — Tutorials', channel: 'AppDateGH' },
      { id: 'uPCftkZkUzI', title: 'Video 4 — Live Set', channel: 'DebeatzGH' }
    ];

    let player, currentIndex = 0, isPlaying = false;
    const playBtn = document.getElementById('playBtn');
    const prevBtn = document.getElementById('prevBtn');
    const nextBtn = document.getElementById('nextBtn');
    const pipBtn = document.getElementById('pipBtn');
    const fsBtn = document.getElementById('fsBtn');
    const muteBtn = document.getElementById('muteBtn');
    const sleepBtn = document.getElementById('sleepBtn');
    const qualitySelect = document.getElementById('qualitySelect');
    const seekFill = document.getElementById('seek-fill');
    const playlistEl = document.getElementById('playlist');

    const mini = document.getElementById('miniPlayer');
    const miniPlay = document.getElementById('miniPlay');
    const miniPrev = document.getElementById('miniPrev');
    const miniNext = document.getElementById('miniNext');
    const miniClose = document.getElementById('miniClose');

    // Render playlist
    function renderPlaylist() {
      playlistEl.innerHTML = '';
      PLAYLIST.forEach((item, i) => {
        const el = document.createElement('div');
        el.className = 'track' + (i === currentIndex ? ' active' : '');
        el.tabIndex = 0;
        el.setAttribute('role','listitem');
        el.innerHTML = `
          <div class="thumb" aria-hidden="true"><img decoding="async" src="https://i.ytimg.com/vi/${item.id}/mqdefault.jpg" alt="" style="width:100%;height:100%;object-fit:cover;border-radius:6px"></div>
          <div style="flex:1">
            <div style="font-weight:800">${item.title}</div>
            <div class="small">${item.channel}</div>
          </div>
        `;
        el.addEventListener('click', ()=> loadIndex(i));
        el.addEventListener('keydown', (ev)=> { if(ev.key==='Enter') loadIndex(i); });
        playlistEl.appendChild(el);
      });
    }

    // YouTube API ready
    function onYouTubeIframeAPIReady() {
      player = new YT.Player('player-frame', {
        height: '100%',
        width: '100%',
        videoId: PLAYLIST[currentIndex].id,
        playerVars: {
          autoplay: 0,
          controls: 1,
          modestbranding: 1,
          rel: 0,
          enablejsapi: 1,
          playsinline: 1
        },
        events: {
          onReady: onPlayerReady,
          onStateChange: onPlayerStateChange,
          onError: (e)=> console.error('YT error', e)
        }
      });
    }

    function onPlayerReady() {
      // move placeholder out since YT injects iframe
      const ph = document.getElementById('player-placeholder');
      if(ph) ph.remove();
      updateNow();
      renderPlaylist();
      bindUI();
      applyPersistedTheme();
      // Restore last index
      const saved = localStorage.getItem('appdate-current');
      if(saved) { currentIndex = parseInt(saved,10) % PLAYLIST.length; player.loadVideoById(PLAYLIST[currentIndex].id); updateNow(); renderPlaylist(); }
    }

    function onPlayerStateChange(event) {
      const state = event.data;
      if(state === YT.PlayerState.PLAYING) { isPlaying=true; playBtn.innerHTML = '<i class="fa fa-pause"></i><span class="small">Pause</span>'; document.getElementById('playBtn').setAttribute('aria-pressed','true'); miniPlay.innerHTML='<i class="fa fa-pause"></i>'; }
      else { isPlaying=false; playBtn.innerHTML = '<i class="fa fa-play"></i><span class="small">Play</span>'; document.getElementById('playBtn').setAttribute('aria-pressed','false'); miniPlay.innerHTML='<i class="fa fa-play"></i>'; }
      if(state === YT.PlayerState.ENDED) { nextIndex(); }
      // update progress
      if(state === YT.PlayerState.PLAYING) startProgress(); else stopProgress();
    }

    // progress updater
    let progressTimer = null;
    function startProgress(){
      stopProgress();
      progressTimer = setInterval(()=> {
        if(!player || !player.getDuration) return;
        const d = player.getDuration();
        if(d>0) {
          const cur = player.getCurrentTime();
          const pct = Math.min(100, (cur/d)*100);
          seekFill.style.width = pct + '%';
        }
      }, 600);
    }
    function stopProgress(){ if(progressTimer) { clearInterval(progressTimer); progressTimer=null; } }

    // Controls
    function togglePlay() {
      if(!player) return;
      const state = player.getPlayerState();
      if(state === YT.PlayerState.PLAYING) player.pauseVideo(); else player.playVideo();
    }
    function prevIndex() {
      currentIndex = (currentIndex - 1 + PLAYLIST.length) % PLAYLIST.length;
      loadIndex(currentIndex);
    }
    function nextIndex() {
      currentIndex = (currentIndex + 1) % PLAYLIST.length;
      loadIndex(currentIndex);
    }
    function loadIndex(i) {
      currentIndex = i;
      localStorage.setItem('appdate-current', currentIndex);
      player.loadVideoById(PLAYLIST[currentIndex].id);
      updateNow();
      renderPlaylist();
    }
    function updateNow() {
      const cur = PLAYLIST[currentIndex];
      document.getElementById('nowTitle').textContent = cur.title;
      document.getElementById('nowChannel').textContent = cur.channel;
      document.getElementById('miniTitle').textContent = cur.title;
      document.getElementById('miniChannel').textContent = cur.channel;
      document.getElementById('miniThumb').innerHTML = `<img decoding="async" src="https://i.ytimg.com/vi/${cur.id}/mqdefault.jpg" alt="" style="width:100%;height:100%;object-fit:cover;border-radius:6px">`;
      // update active class on playlist
      renderPlaylist();
    }

    // UI binding
    function bindUI(){
      playBtn.addEventListener('click', togglePlay);
      prevBtn.addEventListener('click', prevIndex);
      nextBtn.addEventListener('click', nextIndex);
      pipBtn.addEventListener('click', toggleMiniMode);
      fsBtn.addEventListener('click', ()=> {
        const iframe = player.getIframe();
        if(document.fullscreenElement) document.exitFullscreen(); else iframe.requestFullscreen().catch(()=>{});
      });
      muteBtn.addEventListener('click', ()=> {
        if(player.isMuted()) { player.unMute(); muteBtn.innerHTML='<i class="fa fa-volume-up"></i>'; } else { player.mute(); muteBtn.innerHTML='<i class="fa fa-volume-mute"></i>'; }
      });
      qualitySelect.addEventListener('change', ()=> {
        const val = qualitySelect.value;
        if(!val) player.setPlaybackQuality('default'); else player.setPlaybackQuality(val);
      });

      miniPlay.addEventListener('click', ()=> { togglePlay(); });
      miniPrev.addEventListener('click', prevIndex);
      miniNext.addEventListener('click', nextIndex);
      miniClose.addEventListener('click', ()=> { hideMiniMode(); });

      // keyboard shortcuts
      window.addEventListener('keydown', (e)=>{
        if(['INPUT','TEXTAREA'].includes(document.activeElement.tagName)) return;
        if(e.code === 'Space') { e.preventDefault(); togglePlay(); }
        if(e.key === 'n' || e.key === 'N') nextIndex();
        if(e.key === 'p' || e.key === 'P') prevIndex();
        if(e.key === 'm' || e.key === 'M') muteBtn.click();
        if(e.key === 'ArrowUp') { const v = Math.min(100, (player.getVolume()||50)+5); player.setVolume(v); }
        if(e.key === 'ArrowDown') { const v = Math.max(0, (player.getVolume()||50)-5); player.setVolume(v); }
      });
    }

    // Mini player (pin) behavior
    function toggleMiniMode(){
      const isHidden = mini.classList.contains('hidden');
      if(isHidden) showMiniMode(); else hideMiniMode();
    }
    function showMiniMode(){
      mini.classList.remove('hidden'); mini.setAttribute('aria-hidden','false');
      // ensure player keeps playing in background; we don't swap iframe due to cross-origin
    }
    function hideMiniMode(){
      mini.classList.add('hidden'); mini.setAttribute('aria-hidden','true');
    }

    // Sleep timer
    let sleepTimer = null, sleepRemaining = 0, sleepInterval = null;
    sleepBtn.addEventListener('click', ()=> {
      if(sleepTimer){ clearSleepTimer(); } else startSleepTimer(15); // default 15 minutes
    });
    function startSleepTimer(minutes){
      sleepRemaining = minutes*60;
      sleepBtn.textContent = `Sleep ${minutes}m`;
      sleepTimer = setTimeout(()=> { player.pauseVideo(); clearSleepTimer(); }, sleepRemaining*1000);
      sleepInterval = setInterval(()=> {
        sleepRemaining--; if(sleepRemaining<=0) clearSleepTimer();
        sleepBtn.textContent = `Ends ${Math.ceil(sleepRemaining/60)}m`;
      },1000);
    }
    function clearSleepTimer(){
      clearTimeout(sleepTimer); clearInterval(sleepInterval); sleepTimer = null; sleepInterval=null;
      sleepBtn.textContent = 'Sleep Off';
    }

    // Quality - try to set, but YouTube may return available qualities
    // Theme & color dock
    function applyAccent(){ document.documentElement.style.setProperty('--accent','#FF2D55'); document.documentElement.style.setProperty('--brand-glow','rgba(255,45,85,0.1)'); localStorage.setItem('app-theme','accent'); }
    function applyCyan(){ document.documentElement.style.setProperty('--accent','#00f2ff'); document.documentElement.style.setProperty('--brand-glow','rgba(0,242,255,0.08)'); localStorage.setItem('app-theme','cyan'); }
    function applyGreen(){ document.documentElement.style.setProperty('--accent','#1DB954'); document.documentElement.style.setProperty('--brand-glow','rgba(29,185,84,0.06)'); localStorage.setItem('app-theme','green'); }
    function applyPersistedTheme(){ const t=localStorage.getItem('app-theme')||'accent'; if(t==='cyan') applyCyan(); else if(t==='green') applyGreen(); else applyAccent(); }

    // Blog fetching (fixed feed URL). Using Blogger JSON feed
    (function loadBlog(){
      const blogPosts = document.getElementById('blog-posts');
      const blogFeed = 'https://appdategh.blogspot.com/feeds/posts/default?alt=json&max-results=4';
      fetch(blogFeed).then(r=>r.ok? r.json(): Promise.reject(r)).then(data=>{
        const entries = data.feed && data.feed.entry ? data.feed.entry : [];
        if(!entries.length) { blogPosts.innerHTML = '<div class="small">No posts found.</div>'; return; }
        blogPosts.innerHTML = '';
        entries.forEach(e=>{
          const title = e.title.$t || 'Untitled';
          const link = (e.link || []).find(l=>l.rel==='alternate')?.href || '#';
          const snippet = (e.content && e.content.$t ? e.content.$t : (e.summary && e.summary.$t ? e.summary.$t : '')).replace(/<[^>]*>/g,'').slice(0,120) + '…';
          const thumb = (e.media$thumbnail && e.media$thumbnail.url) || 'https://via.placeholder.com/160x90?text=Post';
          const item = document.createElement('a');
          item.className = 'post-item';
          item.href = link; item.target = '_blank'; item.rel='noopener';
          item.innerHTML = `<div class="post-thumb"><img decoding="async" src="${thumb}" alt="" style="width:100%;height:100%;object-fit:cover;border-radius:8px"></div>
            <div class="post-meta"><strong>${title}</strong><div class="small">${snippet}</div></div>`;
          blogPosts.appendChild(item);
        });
      }).catch(err=>{
        blogPosts.innerHTML = '<div class="small">Failed to load posts. Try again later.</div>';
        console.error('Blog fetch error', err);
      });
    })();

    // Persist / hydration
    window.addEventListener('beforeunload', ()=> {
      localStorage.setItem('appdate-current', currentIndex);
    });

    // UI helpers for interop with YT
    // When YT injects iframe into 'player-frame', it replaces content — but we referenced its element ID
    // Because we pass 'player-frame' as the element ID above, YT will create the <iframe> in that container.
    // The placeholder was removed on ready.

    // Expose a simple API for manual testing
    window.appdate = { loadIndex, nextIndex, prevIndex, togglePlay };

  </script>
</body>
</html>
