<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0"/>
<title>Marvel Watchlist</title>
<style>
*{box-sizing:border-box;margin:0;padding:0;-webkit-tap-highlight-color:transparent}
body{background:#0a0a0f;color:#fff;font-family:-apple-system,sans-serif;min-height:100vh;padding-bottom:80px}
.header{background:linear-gradient(135deg,#0a0a0f,#1a0508,#0a0a0f);border-bottom:2px solid #e0152b;padding:20px 16px 14px;text-align:center;position:sticky;top:0;z-index:99}
.header h1{font-size:28px;font-weight:900;letter-spacing:3px;text-transform:uppercase}
.header h1 span{color:#e0152b}
.header p{font-size:10px;color:#666;letter-spacing:2px;text-transform:uppercase;margin-top:4px}
.prog-wrap{max-width:320px;margin:10px auto 0;background:#1c1c26;border-radius:4px;height:5px;overflow:hidden}
.prog-fill{height:100%;background:linear-gradient(90deg,#e0152b,#c9a227);border-radius:4px;transition:width .4s}
.prog-label{font-size:11px;color:#666;margin-top:6px}
.prog-label b{color:#c9a227}
.filters{display:flex;gap:6px;flex-wrap:wrap;justify-content:center;padding:10px 12px;background:#13131a;border-bottom:1px solid #1e1e2a}
.f-btn{background:#1c1c26;border:1px solid #2a2a38;color:#777;padding:5px 11px;border-radius:20px;font-size:11px;cursor:pointer;font-family:inherit}
.f-btn.active{background:#e0152b;border-color:#e0152b;color:#fff}
.f-btn.active-spider{background:#6a1faa;border-color:#cc3bff;color:#fff}
.content{max-width:700px;margin:0 auto;padding:16px 12px}
.phase{margin-bottom:32px}
.phase-head{display:flex;align-items:center;gap:8px;margin-bottom:10px;padding-bottom:8px;border-bottom:1px solid #1e1e2a}
.phase-label{font-size:17px;font-weight:900;letter-spacing:2px;color:#e0152b;text-transform:uppercase}
.phase-name{font-size:11px;color:#aaa;letter-spacing:1px;text-transform:uppercase}
.phase-count{margin-left:auto;font-size:11px;color:#aaa}
.phase-count b{color:#c9a227}
.phase-note{font-size:10px;color:#555;letter-spacing:1.5px;text-transform:uppercase;margin-bottom:8px;padding-left:10px}
.item{display:flex;align-items:center;gap:10px;padding:10px;border-radius:8px;margin-bottom:2px;cursor:pointer;border:1px solid transparent;user-select:none;-webkit-user-select:none}
.item:active{opacity:.7}
.item.done{background:#1a2e1a;border-color:#2d5a2d;opacity:.75}
.checkbox{width:22px;height:22px;flex-shrink:0;border-radius:50%;border:2px solid #333;display:flex;align-items:center;justify-content:center;background:transparent}
.item.done .checkbox{background:#2d5a2d;border-color:#2d5a2d}
.tick{display:none;color:#fff;font-size:12px;font-weight:900}
.item.done .tick{display:block}
.item-info{flex:1;min-width:0}
.item-title{font-size:14px;font-weight:700;color:#fff;line-height:1.3}
.item.done .item-title{text-decoration:line-through;color:#7ec87e}
.item-meta{display:flex;align-items:center;gap:5px;margin-top:4px;flex-wrap:wrap}
.tag{font-size:10px;padding:2px 7px;border-radius:10px;font-weight:700;text-transform:uppercase}
.tag-movie{background:#1e1e3a;color:#9999ee}
.tag-series{background:#1e2e1e;color:#88cc88}
.tag-special{background:#2e2210;color:#ccaa55}
.tag-essential{background:#3a1010;color:#e0152b}
.tag-spider{background:#1a1030;color:#cc3bff;border:1px solid #6a1faa}
.tag-note{font-size:10px;color:#555;font-style:italic}
.tag-year{font-size:11px;color:#555}
</style>
</head>
<body>
<div class="header">
  <h1>MARVEL <span>WATCHLIST</span></h1>
  <p>Road to Doomsday &amp; Brand New Day</p>
  <div class="prog-wrap"><div class="prog-fill" id="pf"></div></div>
  <div class="prog-label" id="pl"></div>
</div>
<div class="filters" id="filters">
  <button class="f-btn active" data-f="all">Tout</button>
  <button class="f-btn" data-f="unwatched">À voir</button>
  <button class="f-btn" data-f="watched">Vus ✓</button>
  <button class="f-btn" data-f="essential">⭐ Doomsday</button>
  <button class="f-btn" data-f="spider">🕷️ Brand New Day</button>
  <button class="f-btn" data-f="movie">Films</button>
  <button class="f-btn" data-f="series">Séries</button>
</div>
<div class="content" id="app"></div>
<script>
const PHASES=[{phase:"X-MEN UNIVERSE",name:"Fox / Legacy",note:"Canon partiel via multivers",items:[{id:"xm1",title:"X-Men",year:2000,type:"movie"},{id:"xm2",title:"X2 : X-Men United",year:2003,type:"movie"},{id:"xm3",title:"X-Men : L'Affrontement Final",year:2006,type:"movie"},{id:"xm4",title:"X-Men Origins : Wolverine",year:2009,type:"movie"},{id:"xm5",title:"X-Men : Le Commencement",year:2011,type:"movie"},{id:"xm6",title:"Wolverine : Le Combat de l'Immortel",year:2013,type:"movie"},{id:"xm7",title:"X-Men : Days of Future Past",year:2014,type:"movie",essential:1},{id:"xm8",title:"Deadpool",year:2016,type:"movie"},{id:"xm9",title:"X-Men : Apocalypse",year:2016,type:"movie"},{id:"xm10",title:"Logan",year:2017,type:"movie"},{id:"xm11",title:"Deadpool 2",year:2018,type:"movie"},{id:"xm12",title:"X-Men : Dark Phoenix",year:2019,type:"movie"},{id:"xm13",title:"The New Mutants",year:2020,type:"movie"}]},{phase:"PHASE 1",name:"Infinity Saga – Début",items:[{id:"p1_1",title:"Captain America : The First Avenger",year:2011,type:"movie"},{id:"p1_2",title:"Agent Carter – Saison 1",year:2015,type:"series",note:"spin-off 1946"},{id:"p1_3",title:"Agent Carter – Saison 2",year:2016,type:"series"},{id:"p1_4",title:"What If…? – Épisode 1",year:2021,type:"special"},{id:"p1_5",title:"Captain Marvel",year:2019,type:"movie"},{id:"p1_6",title:"Iron Man",year:2008,type:"movie"},{id:"p1_7",title:"What If…? – Épisode 6",year:2021,type:"special"},{id:"p1_8",title:"Iron Man 2",year:2010,type:"movie"},{id:"p1_9",title:"L'Incroyable Hulk",year:2008,type:"movie"},{id:"p1_10",title:"Thor",year:2011,type:"movie"},{id:"p1_11",title:"What If…? – Épisode 7",year:2021,type:"special"},{id:"p1_12",title:"Avengers",year:2012,type:"movie"},{id:"p1_13",title:"What If…? – Épisode 3",year:2021,type:"special"}]},{phase:"PHASE 2",name:"Infinity Saga",items:[{id:"p2_1",title:"Iron Man 3",year:2013,type:"movie"},{id:"p2_2",title:"Agents of S.H.I.E.L.D. – S1",year:2013,type:"series",note:"semi-canon"},{id:"p2_3",title:"Thor : Le Monde des Ténèbres",year:2013,type:"movie"},{id:"p2_4",title:"Agents of S.H.I.E.L.D. – S2",year:2014,type:"series",note:"semi-canon"},{id:"p2_5",title:"Captain America : Le Soldat de l'Hiver",year:2014,type:"movie"},{id:"p2_6",title:"Les Gardiens de la Galaxie",year:2014,type:"movie"},{id:"p2_7",title:"Daredevil – Saison 1",year:2015,type:"series",note:"Netflix / canon"},{id:"p2_8",title:"Les Gardiens de la Galaxie Vol. 2",year:2017,type:"movie"},{id:"p2_9",title:"Avengers : L'Ère d'Ultron",year:2015,type:"movie"},{id:"p2_10",title:"What If…? – Épisode 8",year:2021,type:"special"},{id:"p2_11",title:"Jessica Jones – Saison 1",year:2015,type:"series",note:"Netflix / canon"},{id:"p2_12",title:"Ant-Man",year:2015,type:"movie"}]},{phase:"PHASE 3",name:"Infinity Saga – Fin",items:[{id:"p3_1",title:"Daredevil – Saison 2",year:2016,type:"series",note:"Netflix / canon"},{id:"p3_2",title:"Captain America : Civil War",year:2016,type:"movie",spider:1},{id:"p3_3",title:"Agents of S.H.I.E.L.D. – S3",year:2016,type:"series",note:"semi-canon"},{id:"p3_4",title:"Luke Cage – Saison 1",year:2016,type:"series",note:"Netflix / canon"},{id:"p3_5",title:"Iron Fist – Saison 1",year:2017,type:"series",note:"Netflix / canon"},{id:"p3_6",title:"The Defenders – Saison 1",year:2017,type:"series",note:"Netflix / canon"},{id:"p3_7",title:"The Punisher – Saison 1",year:2017,type:"series",note:"Netflix / canon"},{id:"p3_8",title:"Black Widow",year:2021,type:"movie"},{id:"p3_9",title:"Spider-Man : Homecoming",year:2017,type:"movie",spider:1},{id:"p3_10",title:"Agents of S.H.I.E.L.D. – S4",year:2017,type:"series",note:"semi-canon"},{id:"p3_11",title:"Doctor Strange",year:2016,type:"movie"},{id:"p3_12",title:"Jessica Jones – Saison 2",year:2018,type:"series",note:"Netflix / canon"},{id:"p3_13",title:"What If…? – Épisode 4",year:2021,type:"special"},{id:"p3_14",title:"Black Panther",year:2018,type:"movie"},{id:"p3_15",title:"Luke Cage – Saison 2",year:2018,type:"series",note:"Netflix / canon"},{id:"p3_16",title:"What If…? – Épisode 2",year:2021,type:"special"},{id:"p3_17",title:"Iron Fist – Saison 2",year:2018,type:"series",note:"Netflix / canon"},{id:"p3_18",title:"Daredevil – Saison 3",year:2018,type:"series",note:"Netflix / canon"},{id:"p3_19",title:"Thor : Ragnarok",year:2017,type:"movie"},{id:"p3_20",title:"Ant-Man et la Guêpe",year:2018,type:"movie"},{id:"p3_21",title:"Avengers : Infinity War",year:2018,type:"movie",spider:1},{id:"p3_22",title:"Agents of S.H.I.E.L.D. – S5",year:2018,type:"series",note:"semi-canon"},{id:"p3_23",title:"What If…? – Épisode 5 (Zombies!)",year:2021,type:"special"},{id:"p3_24",title:"The Punisher – Saison 2",year:2019,type:"series",note:"Netflix / canon"},{id:"p3_25",title:"Jessica Jones – Saison 3",year:2019,type:"series",note:"Netflix / canon"},{id:"p3_26",title:"Avengers : Endgame",year:2019,type:"movie",essential:1,spider:1},{id:"p3_27",title:"Spider-Man : Far From Home",year:2019,type:"movie",spider:1}]},{phase:"PHASE 4",name:"Multiverse Saga – Début",items:[{id:"p4_1",title:"WandaVision – Saison 1",year:2021,type:"series",essential:1},{id:"p4_2",title:"Falcon et le Soldat de l'Hiver – S1",year:2021,type:"series"},{id:"p4_3",title:"Loki – Saison 1",year:2021,type:"series",essential:1},{id:"p4_4",title:"What If…? – Épisode 9",year:2021,type:"special"},{id:"p4_5",title:"Shang-Chi et la Légende des Dix Anneaux",year:2021,type:"movie"},{id:"p4_6",title:"Les Éternels",year:2021,type:"movie"},{id:"p4_7",title:"Hawkeye – Saison 1",year:2021,type:"series"},{id:"p4_8",title:"Spider-Man : No Way Home",year:2021,type:"movie",essential:1,spider:1},{id:"p4_9",title:"Moon Knight – Saison 1",year:2022,type:"series"},{id:"p4_10",title:"Doctor Strange dans le Multivers de la Folie",year:2022,type:"movie",essential:1},{id:"p4_11",title:"Ms. Marvel – Saison 1",year:2022,type:"series"},{id:"p4_12",title:"Thor : Love and Thunder",year:2022,type:"movie"},{id:"p4_13",title:"She-Hulk : Avocate – Saison 1",year:2022,type:"series",spider:1},{id:"p4_14",title:"Werewolf by Night",year:2022,type:"special"},{id:"p4_15",title:"Black Panther : Wakanda Forever",year:2022,type:"movie"},{id:"p4_16",title:"Agents of S.H.I.E.L.D. – S6",year:2019,type:"series",note:"semi-canon"},{id:"p4_17",title:"Agents of S.H.I.E.L.D. – S7",year:2020,type:"series",note:"semi-canon"}]},{phase:"PHASE 5",name:"Multiverse Saga",items:[{id:"p5_1",title:"Ant-Man et la Guêpe : Quantumania",year:2023,type:"movie"},{id:"p5_2",title:"Les Gardiens de la Galaxie Vol. 3",year:2023,type:"movie"},{id:"p5_3",title:"Secret Invasion – Saison 1",year:2023,type:"series"},{id:"p5_4",title:"Loki – Saison 2",year:2023,type:"series",essential:1},{id:"p5_5",title:"What If…? – Saison 2",year:2023,type:"series",essential:1},{id:"p5_6",title:"The Marvels",year:2023,type:"movie"},{id:"p5_7",title:"Echo – Saison 1",year:2024,type:"series"},{id:"p5_8",title:"Deadpool & Wolverine",year:2024,type:"movie",essential:1},{id:"p5_9",title:"Agatha All Along – Saison 1",year:2024,type:"series"},{id:"p5_10",title:"What If…? – Saison 3",year:2024,type:"series"},{id:"p5_11",title:"Captain America : Brave New World",year:2025,type:"movie",essential:1},{id:"p5_12",title:"Daredevil : Born Again – Saison 1",year:2025,type:"series",essential:1,spider:1},{id:"p5_13",title:"Thunderbolts*",year:2025,type:"movie",essential:1},{id:"p5_14",title:"Ironheart – Saison 1",year:2025,type:"series"}]},{phase:"PHASE 6",name:"Multiverse Saga – Climax",items:[{id:"p6_1",title:"The Fantastic Four : First Steps",year:2025,type:"movie",essential:1},{id:"p6_4",title:"Daredevil : Born Again – Saison 2",year:2026,type:"series",spider:1,note:"mars 2026"},{id:"p6_5",title:"The Punisher : One Last Kill",year:2026,type:"special",spider:1,note:"mai 2026"},{id:"p6_2",title:"Spider-Man : Brand New Day",year:2026,type:"movie",essential:1,spider:1},{id:"p6_3",title:"Avengers : Doomsday",year:2026,type:"movie",essential:1}]}];
let W=JSON.parse(localStorage.getItem('mv')||'{}');let F='all';
function save(){localStorage.setItem('mv',JSON.stringify(W))}
function toggle(id){W[id]=!W[id];save();render()}
function setFilter(f){F=f;document.querySelectorAll('.f-btn').forEach(b=>{b.classList.remove('active','active-spider');if(b.dataset.f===f)b.classList.add(f==='spider'?'active-spider':'active');});render()}
document.getElementById('filters').addEventListener('click',e=>{const b=e.target.closest('.f-btn');if(b)setFilter(b.dataset.f);});
function matches(item){if(F==='watched')return!!W[item.id];if(F==='unwatched')return!W[item.id];if(F==='essential')return!!item.essential;if(F==='spider')return!!item.spider;if(F==='movie')return item.type==='movie';if(F==='series')return item.type==='series'||item.type==='special';return true;}
function render(){const app=document.getElementById('app');let total=0,done=0;PHASES.forEach(p=>p.items.forEach(i=>{total++;if(W[i.id])done++;}));const pct=total?Math.round(done/total*100):0;document.getElementById('pf').style.width=pct+'%';document.getElementById('pl').innerHTML=`<b>${done}</b> / ${total} vus &nbsp;·&nbsp; <b>${pct}%</b>`;let html='';PHASES.forEach(p=>{const filtered=p.items.filter(matches);if(!filtered.length)return;const pd=p.items.filter(i=>W[i.id]).length;html+=`<div class="phase"><div class="phase-head"><span class="phase-label">${p.phase}</span><span class="phase-name">${p.name}</span><span class="phase-count"><b>${pd}</b>/${p.items.length}</span></div>${p.note?`<div class="phase-note">${p.note}</div>`:''}`; filtered.forEach(item=>{const done=!!W[item.id];const tc=item.type==='movie'?'movie':item.type==='series'?'series':'special';const tl=item.type==='movie'?'Film':item.type==='series'?'Série':'Spécial';html+=`<div class="item${done?' done':''}" onclick="toggle('${item.id}')"><div class="checkbox"><span class="tick">✓</span></div><div class="item-info"><div class="item-title">${item.title}</div><div class="item-meta"><span class="tag-year">${item.year}</span><span class="tag tag-${tc}">${tl}</span>${item.essential?'<span class="tag tag-essential">⭐ Essentiel</span>':''}${item.spider?'<span class="tag tag-spider">🕷️ Brand New Day</span>':''}${item.note?`<span class="tag-note">${item.note}</span>`:''}</div></div></div>`;});html+='</div>';});app.innerHTML=html;}
render();
</script>
</body>
</html>
