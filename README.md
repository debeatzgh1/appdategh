<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Daily Entertainment Quiz | Debeatzgh</title>

<style>
body {
  font-family: system-ui, -apple-system, BlinkMacSystemFont;
  background: linear-gradient(135deg,#0f2027,#203a43,#2c5364);
  color: #fff;
  margin: 0;
  padding: 20px;
}

.quiz-box {
  max-width: 600px;
  margin: auto;
  background: rgba(0,0,0,.4);
  padding: 20px;
  border-radius: 18px;
  box-shadow: 0 15px 30px rgba(0,0,0,.5);
}

h1 {
  text-align: center;
  margin-bottom: 10px;
}

.question {
  font-size: 18px;
  margin: 20px 0;
}

.btns {
  display: flex;
  gap: 12px;
}

button {
  flex: 1;
  padding: 14px;
  border-radius: 14px;
  border: none;
  font-size: 16px;
  font-weight: bold;
  cursor: pointer;
}

.true { background: #16a34a; color:#fff; }
.false { background: #dc2626; color:#fff; }

button:hover { opacity: .9; }

#result, #ai-box {
  display: none;
  margin-top: 20px;
}

.recommendation {
  background: rgba(255,255,255,.1);
  padding: 14px;
  border-radius: 14px;
  margin-bottom: 12px;
}

.media-btn {
  display: inline-block;
  background: #0f9d58;
  color: #fff;
  padding: 12px 16px;
  border-radius: 12px;
  font-weight: 700;
  cursor: pointer;
  margin-top: 8px;
}

.media-btn.youtube {
  background: #ff0000;
}

/* Modal */
#media-modal {
  display: none;
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,.9);
  z-index: 99999;
}
#media-modal iframe {
  width: 100%;
  height: 100%;
  border: none;
}
.close {
  position: absolute;
  top: 12px;
  right: 18px;
  font-size: 30px;
  cursor: pointer;
}
</style>
</head>

<body>

<div class="quiz-box">
  <h1>🎉 Daily Entertainment Quiz</h1>
  <p id="progress"></p>

  <div class="question" id="question"></div>

  <div class="btns">
    <button class="true" onclick="answer(true)">TRUE</button>
    <button class="false" onclick="answer(false)">FALSE</button>
  </div>

  <div id="result"></div>

  <div id="ai-box">
    <h3>🤖 AI Recommendations</h3>
    <div id="ai-results"></div>
  </div>
</div>

<!-- MEDIA MODAL -->
<div id="media-modal">
  <div class="close" onclick="closeMedia()">✕</div>
  <iframe id="media-frame"></iframe>
</div>

<!-- SOUND EFFECTS -->
<audio id="clickSound" src="https://assets.mixkit.co/active_storage/sfx/2568/2568-preview.mp3"></audio>
<audio id="correctSound" src="https://assets.mixkit.co/active_storage/sfx/2019/2019-preview.mp3"></audio>
<audio id="wrongSound" src="https://assets.mixkit.co/active_storage/sfx/209/209-preview.mp3"></audio>
<audio id="finishSound" src="https://assets.mixkit.co/active_storage/sfx/1110/1110-preview.mp3"></audio>

<script>
const questions = [
  {q:"Ghanaian music is popular across Africa.", a:true},
  {q:"Audiomack is only for podcasts.", a:false},
  {q:"YouTube playlists can stream music videos.", a:true},
  {q:"Entertainment quizzes improve engagement.", a:true},
  {q:"Afrobeats originated outside Africa.", a:false}
];

let index = 0;
let score = 0;

const qBox = document.getElementById("question");
const progress = document.getElementById("progress");

function loadQuestion() {
  progress.innerText = `Question ${index+1} of ${questions.length}`;
  qBox.innerText = questions[index].q;
}

function play(id) {
  const s = document.getElementById(id);
  s.currentTime = 0;
  s.play();
}

function answer(val) {
  play("clickSound");

  if (val === questions[index].a) {
    score++;
    play("correctSound");
  } else {
    play("wrongSound");
  }

  index++;
  if (index < questions.length) {
    loadQuestion();
  } else {
    finishQuiz();
  }
}

function finishQuiz() {
  play("finishSound");
  document.querySelector(".btns").style.display = "none";

  document.getElementById("result").style.display = "block";
  document.getElementById("result").innerHTML =
    `<h2>✅ Your Score: ${score}/${questions.length}</h2>`;

  showRecommendations();
}

function showRecommendations() {
  document.getElementById("ai-box").style.display = "block";

  document.getElementById("ai-results").innerHTML = `
    <div class="recommendation">
      🎧 <strong>Music Recommendation</strong><br>
      Enjoy curated mixes from Ghanaian DJs
      <br>
      <span class="media-btn"
        onclick="openMedia('https://audiomack.com/debeatz4')">
        ▶ Open Audiomack @debeatz4
      </span>
    </div>

    <div class="recommendation">
      📺 <strong>Video Recommendation</strong><br>
      Trending entertainment playlists
      <br>
      <span class="media-btn youtube"
        onclick="openMedia('https://youtube.com/playlist?list=PLMOQxjh_hNfRbNjMEMpG_wBsf4NODJGEG')">
        ▶ Open YouTube Playlist
      </span>
    </div>
  `;
}

function openMedia(url) {
  document.getElementById("media-frame").src = url;
  document.getElementById("media-modal").style.display = "block";
}

function closeMedia() {
  document.getElementById("media-frame").src = "";
  document.getElementById("media-modal").style.display = "none";
}

loadQuestion();
</script>

</body>
</html>
