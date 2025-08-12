<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<title>おみくじアプリ最終版</title>
<style>
  body {
    font-family: "Hiragino Kaku Gothic ProN", sans-serif;
    text-align: center;
    margin-top: 50px;
    transition: background-color 0.5s;
  }
  h1 {
    font-size: 2em;
    margin-bottom: 30px;
  }
  button {
    padding: 10px 20px;
    font-size: 1.2em;
    cursor: pointer;
    background-color: #ff6347;
    border: none;
    border-radius: 8px;
    color: white;
  }
  button:hover {
    background-color: #e5533d;
  }
  button:disabled {
    background-color: gray;
    cursor: not-allowed;
  }
  #result {
    margin-top: 30px;
    font-size: 2.5em;
    font-weight: bold;
  }
  #message {
    margin-top: 20px;
    font-size: 1.2em;
    color: #fff;
  }
  #image {
    margin-top: 20px;
    width: 150px;
    height: auto;
  }
</style>
</head>
<body>

<h1>おみくじ</h1>
<button id="drawBtn" onclick="drawOmikuji()">引く！</button>

<div id="result"></div>
<img id="image" src="" alt="" style="display:none;">
<div id="message"></div>

<script>
// 運勢データ
const fortunes = [
  {
    text: "大吉",
    color: "red",
    bg: "#ffcccc",
    img: "https://i.imgur.com/jQ7PcdU.png",
    msg: "今日は最高の一日になりそう！",
    sound: "https://actions.google.com/sounds/v1/cartoon/clang_and_wobble.ogg"
  },
  {
    text: "中吉",
    color: "orange",
    bg: "#ffe4b5",
    img: "https://i.imgur.com/9sz0V0L.png",
    msg: "まずまず順調。チャンスは逃さないで！",
    sound: "https://actions.google.com/sounds/v1/cartoon/wood_plank_flicks.ogg"
  },
  {
    text: "小吉",
    color: "green",
    bg: "#ccffcc",
    img: "https://i.imgur.com/f4eZyXb.png",
    msg: "小さな幸せに気づける日。",
    sound: "https://actions.google.com/sounds/v1/cartoon/wood_twang.ogg"
  },
  {
    text: "末吉",
    color: "blue",
    bg: "#cce5ff",
    img: "https://i.imgur.com/7k6V76B.png",
    msg: "焦らずコツコツ進めましょう。",
    sound: "https://actions.google.com/sounds/v1/cartoon/siren_whistle.ogg"
  }
];

// ページ読み込み時に今日引いたか確認
window.onload = function() {
  const lastDate = localStorage.getItem("omikujiDate");
  const today = new Date().toDateString();

  if (lastDate === today) {
    // すでに引いている場合は結果を表示してボタン無効化
    const savedIndex = localStorage.getItem("omikujiIndex");
    showResult(fortunes[savedIndex]);
    document.getElementById("drawBtn").disabled = true;
  }
};

function drawOmikuji() {
  const today = new Date().toDateString();
  const randomIndex = Math.floor(Math.random() * fortunes.length);
  const selected = fortunes[randomIndex];

  // 結果を表示
  showResult(selected);

  // 日付と結果を保存
  localStorage.setItem("omikujiDate", today);
  localStorage.setItem("omikujiIndex", randomIndex);

  // ボタン無効化
  document.getElementById("drawBtn").disabled = true;
}

function showResult(selected) {
  const resultEl = document.getElementById("result");
  const imageEl = document.getElementById("image");
  const messageEl = document.getElementById("message");

  resultEl.textContent = selected.text;
  resultEl.style.color = selected.color;
  document.body.style.backgroundColor = selected.bg;

  imageEl.src = selected.img;
  imageEl.style.display = "block";

  messageEl.textContent = selected.msg;

  // 効果音再生
  const audio = new Audio(selected.sound);
  audio.play();
}
</script>

</body>
</html>
