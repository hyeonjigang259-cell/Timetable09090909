# Timetable09090909
<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>실시간 시간표</title>
<link rel="manifest" href="manifest.json">
<link href="https://cdn.jsdelivr.net/gh/sunn-us/SUIT/fonts/static/woff2/SUIT.css" rel="stylesheet">
<meta name="theme-color" content="#3b82f6">
<style>
body{font-family:SUIT,sans-serif;background:#fff;color:#111;margin:0;padding:20px}
.card{max-width:900px;margin:auto}
.box{border:1px solid #ddd;border-radius:16px;padding:16px;margin:12px 0}
.current{background:#3b82f6;color:#fff}
button{background:#3b82f6;color:#fff;border:none;padding:8px 12px;border-radius:8px}
input{padding:6px;margin:4px}
</style>
</head>
<body>
<div class="card">
<h1>📚 실시간 시간표</h1>
<div id="now"></div>
<div class="box">
<h2 id="period">현재 교시</h2>
<div id="subject"></div>
<div id="teacher"></div>
<div id="room"></div>
<div id="countdown"></div>
</div>
<div class="box">
<h3>시험 일정</h3>
<div id="dday"></div>
<div id="suneung"></div>
<div id="remain"></div>
</div>
<div class="box">
<h3>오늘 시간표</h3>
<div id="today"></div>
</div>
<div class="box">
<h3>시간표 편집</h3>
<button onclick="editData()">편집</button>
</div>
</div>
<script>
let timetable=JSON.parse(localStorage.getItem("timetable")||'{"월":{"1교시":{"subject":"국어","teacher":"선생님","room":"3-1"}}}');
const periods=[["1교시","08:40","09:30"],["2교시","09:40","10:30"],["3교시","10:40","11:30"],["4교시","11:40","12:30"],["점심시간","12:30","13:30"],["5교시","13:30","14:20"],["6교시","14:30","15:20"],["청소시간","15:20","15:40"],["7교시","15:40","16:30"]];
function mins(t){let[a,b]=t.split(":").map(Number);return a*60+b}
function currentPeriod(){
 let n=new Date(),m=n.getHours()*60+n.getMinutes();
 for(let p of periods){if(m>=mins(p[1])&&m<mins(p[2])) return p}
 return null
}
function update(){
 let n=new Date();
 let day=["일","월","화","수","목","금","토"][n.getDay()];
 document.getElementById("now").innerText=n.toLocaleString("ko-KR");
 let cp=currentPeriod();
 let today=document.getElementById("today"); today.innerHTML="";
 periods.forEach(p=>{
  let d=document.createElement("div");
  d.textContent=p[0];
  if(cp&&cp[0]==p[0]) d.className="current";
  today.appendChild(d);
 });
 if(cp){
   document.getElementById("period").innerText=cp[0];
   let info=(timetable[day]||{})[cp[0]]||{};
   subject.innerText="과목: "+(info.subject||"-");
   teacher.innerText="선생님: "+(info.teacher||"-");
   room.innerText="교실: "+(info.room||"-");
   let end=mins(cp[2]);
   let nowm=n.getHours()*60+n.getMinutes();
   let diff=end-nowm;
   countdown.innerText="다음까지 약 "+diff+"분";
 } else countdown.innerText="수업 시간 아님";
 let exam=new Date("2026-07-09");
 dday.innerText="기말고사 D-"+Math.ceil((exam-n)/(1000*60*60*24));
 let csat=new Date("2026-11-12");
 suneung.innerText="수능 D-"+Math.ceil((csat-n)/(1000*60*60*24));
 let endday=new Date(); endday.setHours(16,30,0,0);
 remain.innerText="하교까지 "+Math.max(0,Math.floor((endday-n)/60000))+"분";
}
function editData(){
 let day=prompt("요일 입력(월~금)");
 let period=prompt("교시 입력");
 let subject=prompt("과목");
 let teacher=prompt("선생님");
 let room=prompt("교실");
 timetable[day]=timetable[day]||{};
 timetable[day][period]={subject,teacher,room};
 localStorage.setItem("timetable",JSON.stringify(timetable));
 update();
}
update(); setInterval(update,10000);
if('serviceWorker' in navigator){navigator.serviceWorker.register('service-worker.js');}
</script>
</body>
</html>
