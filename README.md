<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Debeatzgh Entertainment Hub</title>

<style>
body{
  margin:0;
  font-family:"Segoe UI",system-ui,sans-serif;
  background:linear-gradient(135deg,#0f172a,#020617);
  color:#fff;
}

/* Header */
.header{
  text-align:center;
  padding:40px 20px 20px;
}
.header h1{font-size:2rem;margin-bottom:10px;}
.header p{color:#cbd5f5;max-width:720px;margin:auto;}

/* Grid */
.grid{
  display:grid;
  grid-template-columns:repeat(auto-fit,minmax(260px,1fr));
  gap:24px;
  padding:30px;
  max-width:1200px;
  margin:auto;
}

/* Card */
.card{
  background:rgba(255,255,255,.06);
  border-radius:18px;
  overflow:hidden;
  box-shadow:0 10px 35px rgba(0,0,0,.4);
  transition:.3s;
}
.card:hover{transform:translateY(-6px);}
.card img{
  width:100%;
  height:160px;
  object-fit:cover;
}
.card-content{padding:18px;}
.card-content h3{font-size:1.2rem;margin-bottom:8px;}
.card-content p{font-size:.9rem;color:#dbeafe;margin-bottom:14px;}

/* Buttons */
.card-btn{
  display:inline-block;
  background:linear-gradient(135deg,#22c55e,#16a34a);
  padding:10px 16px;
  border-radius:999px;
  font-size:.85rem;
  font-weight:600;
  cursor:pointer;
  margin-right:8px;
  box-shadow:0 6px 20px rgba(0,0,0,.35);
}

/* Modal */
#modal{
  display:none;
  position:fixed;
  inset:0;
  background:rgba(0,0,0,.85);
  z-index:9999;
}
.modal-box{
  width:96%;
  height:92%;
  margin:3% auto;
  background:#000;
  border-radius:20px;
  position:relative;
  overflow:hidden;
}
.modal-box iframe{width:100%;height:100%;border:none;}
.close{
  position:absolute;
  top:10px;
  right:16px;
  font-size:28px;
  cursor:pointer;
  color:#22c55e;
  z-index:20;
}

/* Overlay controls */
.overlay-controls{
  position:absolute;
  bottom:18px;
  right:18px;
  display:flex;
  gap:10px;
  z-index:20;
}
.overlay-btn{
  background:rgba(0,0,0,.7);
  border:1px solid rgba(255,255,255,.15);
  padding:10px 14px;
  border-radius:999px;
  font-size:.8rem;
  cursor:pointer;
}
.subscribe-btn{
  background:linear-gradient(135deg,#ef4444,#dc2626);
  font-weight:600;
}

/* Floating Mini Player */
#miniPlayer{
  display:none;
  position:fixed;
  bottom:16px;
  right:16px;
  width:280px;
  height:158px;
  background:#000;
  border-radius:14px;
  overflow:hidden;
  z-index:99999;
  box-shadow:0 12px 40px rgba(0,0,0,.6);
}
#miniPlayer iframe{width:100%;height:100%;border:none;}
.mini-controls{
  position:absolute;
  top:6px;
  right:6px;
  display:flex;
  gap:6px;
  z-index:10;
}
.mini-btn{
  background:rgba(0,0,0,.7);
  color:#fff;
  font-size:12px;
  padding:4px 8px;
  border-radius:999px;
  cursor:pointer;
}
</style>
</head>

<body>

<div class="header">
  <h1>🎭 Debeatzgh Entertainment Hub</h1>
  <p>Music, videos, playlists, tutorials, and games — all in one immersive experience.</p>
</div>

<div class="grid">

  <!-- Audiomack -->
  <div class="card">
    <img src="https://debeatzgh.wordpress.com/wp-content/uploads/2025/08/wp-17550752905574451607135626005441.jpg">
    <div class="card-content">
      <h3>🎵 Audiomack Streams</h3>
      <p>Curated beats, chill vibes, and creative soundscapes.</p>
      <div class="card-btn" onclick="openFrame('https://debeatzgh1.github.io/tracks/')">Play Music</div>
    </div>
  </div>

  <!-- YouTube Channel -->
  <div class="card">
    <img src="https://debeatzgh.wordpress.com/wp-content/uploads/2025/08/createamoderntech-inspiredlogoforadigitalcontenthubcalledappdategh4933013559151235986.jpg">
    <div class="card-content">
      <h3>📺 YouTube Channel</h3>
      <p>Original videos, creativity, and digital entertainment.</p>
      <div class="card-btn" onclick="openFrame('https://www.youtube.com/embed/@debeatzgh')">Visit Channel</div>
    </div>
  </div>

  <!-- Entertainment Playlist -->
  <div class="card">
    <img src="https://debeatzgh.wordpress.com/wp-content/uploads/2025/08/wp-17550417028037724303471824316179.jpg">
    <div class="card-content">
      <h3>🎬 Entertainment Playlist</h3>
      <p>Handpicked entertainment and creative videos.</p>
      <div class="card-btn" onclick="openFrame('https://www.youtube.com/embed/videoseries?list=PLMOQxjh_hNfRbNjMEMpG_wBsf4NODJGEG')">Watch Playlist</div>
    </div>
  </div>

  <!-- Tutoring Playlist -->
  <div class="card">
    <img src="https://debeatzgh.wordpress.com/wp-content/uploads/2025/12/screenshot_20251215-234743_17563327224807958415.png">
    <div class="card-content">
      <h3>📘 Tutorials & Learning</h3>
      <p>Educational tutorials and step-by-step learning content.</p>
      <div class="card-btn" onclick="openFrame('https://www.youtube.com/embed/videoseries?list=PLMOQxjh_hNfSkZyV_sjQfiHpsHDpHjb1u')">Watch Tutorials</div>
    </div>
  </div>

  <!-- Games -->
  <div class="card">
    <img src="https://debeatzgh.wordpress.com/wp-content/uploads/2025/08/wp-17550752768858270075844257043940.jpg">
    <div class="card-content">
      <h3>🎮 Games & Quizzes</h3>
      <p>Play interactive games and quizzes for fun and learning.</p>
      <div class="card-btn" onclick="openFrame('https://debeatzgh1.github.io/games-/')">Play Games</div>
    </div>
  </div>

</div>

<!-- Modal -->
<div id="modal">
  <div class="modal-box" id="modalBox">
    <span class="close" onclick="closeFrame()">✖</span>
    <div class="overlay-controls">
      <div class="overlay-btn subscribe-btn" onclick="subscribeYT()">🔔 Subscribe</div>
      <div class="overlay-btn" onclick="fullscreen()">⛶ Fullscreen</div>
    </div>
    <iframe id="frame" allowfullscreen></iframe>
  </div>
</div>

<!-- Mini Player -->
<div id="miniPlayer">
  <div class="mini-controls">
    <div class="mini-btn" onclick="restorePlayer()">⛶</div>
    <div class="mini-btn" onclick="closeMini()">✖</div>
  </div>
  <iframe id="miniFrame" allowfullscreen></iframe>
</div>

<script>
function openFrame(url){
  document.getElementById("frame").src=url;
  document.getElementById("modal").style.display="block";
  closeMini();
}

function closeFrame(){
  const src=document.getElementById("frame").src;
  document.getElementById("modal").style.display="none";
  document.getElementById("frame").src="";
  if(src.includes("youtube")){
    document.getElementById("miniFrame").src=src;
    document.getElementById("miniPlayer").style.display="block";
  }
}

function restorePlayer(){
  document.getElementById("modal").style.display="block";
  document.getElementById("frame").src=document.getElementById("miniFrame").src;
  closeMini();
}

function closeMini(){
  document.getElementById("miniPlayer").style.display="none";
  document.getElementById("miniFrame").src="";
}

function subscribeYT(){
  window.open("https://youtube.com/@debeatzgh?sub_confirmation=1","_blank");
}

function fullscreen(){
  const box=document.getElementById("modalBox");
  if(box.requestFullscreen) box.requestFullscreen();
}
</script>

</body>
</html>
