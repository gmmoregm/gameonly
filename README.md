# mygame
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Gaming Video Hub</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<style>
body { margin:0; font-family:Arial,sans-serif; background:linear-gradient(120deg,#8e44ad,#c39bd3); color:white; }
header{ padding:30px 0; text-align:center;font-size:36px; text-shadow:2px 2px 5px rgba(0,0,0,0.4);}
.search-bar {text-align:center; margin-bottom:20px;}
.search-bar input {padding:10px; width:300px; border-radius:10px; border:none; font-size:16px;}
.videos{ display:grid; grid-template-columns:repeat(auto-fit,minmax(320px,1fr)); gap:20px; padding:20px; }
.card{ background: rgba(255,255,255,0.1); border-radius:15px; overflow:hidden; text-align:center; transition: transform 0.3s, background 0.3s; }
.card:hover{ transform: scale(1.05); background: rgba(255,255,255,0.2); }
.card h3{ margin:10px 5px; font-size:18px; }
.card iframe{ width:100%; height:180px; border:none; }
@media(max-width:600px){ .card { width:90%; margin:auto; } .card iframe{ height:200px;} .search-bar input{width:90%;} }
</style>
</head>
<body>

<header>🎮 Gaming Video Hub</header>

<div class="search-bar">
  <input type="text" id="searchInput" placeholder="Search videos by keyword...">
</div>

<div class="videos" id="videoContainer"></div>

<script>
// 🔹 1. შეცვალე შენი YouTube API Key
const API_KEY = "YOUR_YOUTUBE_API_KEY"; // ← შეცვალე აქ
const MAX_RESULTS = 50; // უკანასკნელი 50 ვიდეო
const CATEGORY_ID = 20; // Gaming category

let allVideos = []; // შემდგომში გამოიყენება search-ისთვის

// 🔹 2. Fetch latest Gaming videos
fetch(`https://www.googleapis.com/youtube/v3/search?part=snippet&maxResults=${MAX_RESULTS}&order=date&type=video&videoCategoryId=${CATEGORY_ID}&key=${API_KEY}`)
.then(res => res.json())
.then(data => {
    allVideos = data.items.filter(item => item.id.kind === "youtube#video");
    displayVideos(allVideos);
})
.catch(err => console.error("YouTube API error:", err));

// 🔹 3. Display function
function displayVideos(videos){
    const container = document.getElementById("videoContainer");
    container.innerHTML = "";
    videos.forEach(item=>{
        const videoId = item.id.videoId;
        const title = item.snippet.title;
        const card = document.createElement("div");
        card.className = "card";
        card.innerHTML = `<iframe src="https://www.youtube.com/embed/${videoId}" allowfullscreen></iframe><h3>${title}</h3>`;
        container.appendChild(card);
    });
}

// 🔹 4. Search / Filter functionality
const searchInput = document.getElementById("searchInput");
searchInput.addEventListener("input", e => {
    const query = e.target.value.toLowerCase();
    const filtered = allVideos.filter(item => item.snippet.title.toLowerCase().includes(query));
    displayVideos(filtered);
});
</script>

<footer style="text-align:center; margin-top:50px; opacity:0.7; padding:20px;">
© 2026 Gaming Video Hub
</footer>

</body>
</html>
