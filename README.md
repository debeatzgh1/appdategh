<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Debeatzgh Entertainment Hub</title>

<style>
body {
  margin: 0;
  font-family: "Segoe UI", system-ui, sans-serif;
  background: linear-gradient(135deg,#0f172a,#020617);
  color: #fff;
}

/* Header */
.header {
  text-align: center;
  padding: 40px 20px 20px;
}
.header h1 {
  font-size: 2rem;
  margin-bottom: 10px;
}
.header p {
  color: #cbd5f5;
  max-width: 720px;
  margin: auto;
}

/* Grid */
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(260px,1fr));
  gap: 24px;
  padding: 30px;
  max-width: 1200px;
  margin: auto;
}

/* Card */
.card {
  background: rgba(255,255,255,.06);
  border-radius: 18px;
  overflow: hidden;
  box-shadow: 0 10px 35px rgba(0,0,0,.4);
  transition: transform .3s ease;
}
.card:hover { transform: translateY(-6px); }

.card img {
  width: 100%;
  height: 160px;
  object-fit: cover;
}

.card-content {
  padding: 18px;
}
.card-content h3 {
  font-size: 1.2rem;
  margin-bottom: 8px;
}
.card-content p {
  font-size: .9rem;
  color: #dbeafe;
  margin-bottom: 14px;
}

/* Button */
.card-btn {
  display: inline-block;
  background: linear-gradient(135deg,#22c55e,#16a34a);
  padding: 10px 16px;
  border-radius: 999px;
  font-size: .85rem;
  font-weight: 600;
  cursor: pointer;
  box-shadow: 0 6px 20px rgba(0,0,0,.35);
}

/* Modal */
#modal {
  display: none;
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,.85);
  z-index: 9999;
}

.modal-box {
  width: 96%;
  height: 92%;
  margin: 3% auto;
  background: #000;
  border-radius: 20px;
  overflow: hidden;
  position: relative;
}

.modal-box iframe {
  width: 100%;
  height: 100%;
  border: none;
}

.close {
  position: absolute;
  top: 10px;
  right: 16px;
  font-size: 28px;
  cursor: pointer;
  color: #22c55e;
  z-index: 10;
}
</style>
</head>

<body>

<!-- HEADER -->
<div class="header">
  <h1>🎭 Debeatzgh Entertainment Hub</h1>
  <p>Explore blog updates, music, games, and video content in one fun and interactive space.</p>
</div>

<!-- GRID -->
<div class="grid">

  <!-- Blog -->
  <div class="card">
    <img src="https://debeatzgh.wordpress.com/wp-content/uploads/2025/12/screenshot_20251215-234743_17563327224807958415.png">
    <div class="card-content">
      <h3>📰 Blog Updates</h3>
      <p>Latest trends, digital stories, entertainment updates and creative tech news.</p>
      <div class="card-btn" onclick="openFrame('https://appdategh.blogspot.com/')">Read Blog</div>
    </div>
  </div>

  <!-- Music -->
  <div class="card">
    <img src="https://debeatzgh.wordpress.com/wp-content/uploads/2025/08/wp-17550417188267308669484942620808.jpg">
    <div class="card-content">
      <h3>🎵 Audiomack Tracks</h3>
      <p>Enjoy curated beats, study music, chill vibes and creative soundscapes.</p>
      <div class="card-btn" onclick="openFrame('https://debeatzgh1.github.io/tracks/')">Play Music</div>
    </div>
  </div>

  <!-- Games -->
  <div class="card">
    <img src="https://debeatzgh.wordpress.com/wp-content/uploads/2025/08/wp-17550752768858270075844257043940.jpg">
    <div class="card-content">
      <h3>🎮 Games & Quizzes</h3>
      <p>Interactive games and quizzes for fun, learning, and entertainment.</p>
      <div class="card-btn" onclick="openFrame('https://debeatzgh1.github.io/games-/')">Play Games</div>
    </div>
  </div>

  <!-- YouTube -->
  <div class="card">
    <img src="https://debeatzgh.wordpress.com/wp-content/uploads/2025/08/createamoderntech-inspiredlogoforadigitalcontenthubcalledappdategh4933013559151235986.jpg">
    <div class="card-content">
      <h3>📺 Video Playlist</h3>
      <p>Watch curated entertainment and digital creativity videos.</p>
      <div class="card-btn" onclick="openFrame('https://debeatzgh1.github.io/E-Hub-/')">Watch Videos</div>
    </div>
  </div>

</div>

<!-- MODAL -->
<div id="modal">
  <div class="modal-box">
    <span class="close" onclick="closeFrame()">✖</span>
    <iframe id="frame"></iframe>
  </div>
</div>

<script>
function openFrame(url){
  document.getElementById("frame").src = url;
  document.getElementById("modal").style.display = "block";
}

function closeFrame(){
  document.getElementById("modal").style.display = "none";
  document.getElementById("frame").src = "";
}
</script>

</body>
</html>
