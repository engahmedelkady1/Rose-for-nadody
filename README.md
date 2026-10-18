<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Rose Blooming from Code</title>
<meta name="description" content="A 3D rose blooming from pure CSS and JavaScript.">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,500;1,400;1,500&family=Inter:wght@400;500;600&display=swap" rel="stylesheet">
<style>
:root{color-scheme:dark;--bg1:#1b0710;--bg2:#050105;--rose:#e63950;--gold:#f5d7a1;--ink:#fbeff2;--muted:#b9a0a8;
  --glass:rgba(255,255,255,.06);--line:rgba(255,255,255,.14);
  --serif:'Playfair Display',Georgia,serif;--sans:'Inter',system-ui,sans-serif;
  box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}
html{scroll-padding-top:env(safe-area-inset-top,0px)}
*{box-sizing:border-box;margin:0;padding:0}
html,body{height:100%;overflow:hidden}
body{background:radial-gradient(ellipse at 50% 38%,var(--bg1),var(--bg2) 72%);color:var(--ink);font-family:var(--sans);-webkit-font-smoothing:antialiased}
.vignette{position:fixed;inset:0;background:radial-gradient(ellipse at center,transparent 45%,rgba(0,0,0,.65));pointer-events:none;z-index:5}
.spotlight{position:fixed;left:50%;top:-10%;width:70vmin;height:120vh;transform:translateX(-50%);
  background:conic-gradient(from 180deg at 50% 0,transparent 165deg,rgba(255,200,215,.07) 180deg,transparent 195deg);filter:blur(14px);pointer-events:none}
.ambient{position:fixed;left:50%;top:38%;width:90vmin;height:90vmin;margin:-45vmin 0 0 -45vmin;border-radius:50%;
  background:radial-gradient(circle,rgba(230,57,80,.35),transparent 62%);opacity:0;transition:opacity 3s ease;pointer-events:none;filter:blur(20px)}
body.bloomed .ambient{opacity:1;animation:breathe 5s ease-in-out infinite}
@keyframes breathe{50%{transform:scale(1.12);opacity:.75}}
.dust i{position:fixed;bottom:-10px;width:3px;height:3px;border-radius:50%;background:var(--gold);opacity:0;animation:rise linear infinite}
@keyframes rise{10%{opacity:.7}100%{transform:translateY(-110vh) translateX(var(--dx));opacity:0}}

/* intro card */
.overlay{position:fixed;inset:0;display:grid;place-items:center;z-index:20;transition:opacity 1s ease,visibility 1s;padding:20px}
.overlay.hide{opacity:0;visibility:hidden}
.card{position:relative;width:min(340px,100%);padding:36px 30px 30px;text-align:center;border-radius:24px;background:var(--glass);
  border:1px solid var(--line);backdrop-filter:blur(18px);-webkit-backdrop-filter:blur(18px);box-shadow:0 30px 80px rgba(0,0,0,.55),inset 0 1px 0 rgba(255,255,255,.12)}
.card .icon{font-size:44px;filter:drop-shadow(0 6px 18px rgba(230,57,80,.6));animation:float 3.5s ease-in-out infinite}
@keyframes float{50%{transform:translateY(-6px)}}
.card h2{font-family:var(--serif);font-weight:500;font-size:30px;margin:10px 0 22px;letter-spacing:.02em}
.bar{height:4px;border-radius:4px;background:rgba(255,255,255,.1);overflow:hidden}
.bar i{display:block;height:100%;width:0;border-radius:4px;background:linear-gradient(90deg,#9d0f27,var(--rose),var(--gold));transition:width .25s linear}
.status{margin:12px 0 22px;font-size:12.5px;color:var(--muted);letter-spacing:.06em;min-height:1.2em}
.btn{position:relative;overflow:hidden;width:100%;padding:14px 18px;border:0;border-radius:14px;font:600 15px var(--sans);letter-spacing:.04em;color:#fff;
  background:linear-gradient(135deg,#c81e3a,#8d0c22);box-shadow:0 10px 28px rgba(200,30,58,.45);cursor:pointer;transition:transform .2s,box-shadow .2s,opacity .3s}
.btn:disabled{opacity:.4;cursor:not-allowed;box-shadow:none}
.btn:not(:disabled):hover{transform:translateY(-2px);box-shadow:0 14px 34px rgba(200,30,58,.6)}
.btn:not(:disabled)::after{content:"";position:absolute;top:0;left:-60%;width:40%;height:100%;background:linear-gradient(100deg,transparent,rgba(255,255,255,.35),transparent);transform:skewX(-20deg);animation:shine 2.8s infinite}
@keyframes shine{to{left:130%}}

/* rose */
.scene{position:fixed;inset:0;display:flex;justify-content:center;align-items:flex-start;padding-top:max(6vh,24px)}
.wrap{position:relative;width:0;height:480px;perspective:900px;transform-style:preserve-3d;transition:transform .2s ease-out}
.stem{position:absolute;left:-5px;top:160px;width:10px;height:310px;border-radius:6px;transform:scaleY(0);transform-origin:50% 100%;
  background:linear-gradient(90deg,#0c3a1c,#2a8a45 45%,#0c3a1c);box-shadow:0 0 20px rgba(40,140,70,.25);transition:transform 1.6s cubic-bezier(.3,.7,.2,1)}
.grow .stem{transform:scaleY(1)}
.thorn{position:absolute;width:12px;height:14px;background:#1b6a35;clip-path:polygon(0 0,100% 50%,0 100%);opacity:0;transition:opacity .6s 1.4s}
.t1{left:4px;top:300px;transform:rotate(-20deg)}
.t2{left:-16px;top:380px;transform:scaleX(-1) rotate(-20deg)}
.grow .thorn{opacity:1}
.leaf{position:absolute;top:345px;width:78px;height:38px;transform:scale(0);opacity:0;transition:transform 1.3s cubic-bezier(.3,1.4,.4,1) 1.1s,opacity .5s 1.1s;
  background:linear-gradient(135deg,#2f9a4f,#0f4a25);box-shadow:inset 0 -6px 12px rgba(0,0,0,.35)}
.leaf::after{content:"";position:absolute;left:8%;right:8%;top:50%;height:1px;background:rgba(255,255,255,.22)}
.ll{left:-83px;border-radius:100% 0 100% 0;transform-origin:100% 100%}
.lr{left:5px;border-radius:0 100% 0 100%;transform-origin:0 100%}
.grow .ll{transform:scale(1) rotate(-14deg);opacity:1}
.grow .lr{transform:scale(1) rotate(14deg);opacity:1}
.calyx{position:absolute;left:-26px;top:150px;width:52px;height:30px;border-radius:50% 50% 60% 60%;background:radial-gradient(circle at 50% 30%,#2f9a4f,#0f4a25);transform:scale(0);transition:transform 1s 1.2s}
.grow .calyx{transform:scale(1)}
.head{position:absolute;left:0;top:160px;width:0;height:0;transform-style:preserve-3d;transform:rotateX(-26deg)}
.spin{position:absolute;inset:0;transform-style:preserve-3d}
.bloom .spin{animation:spin 26s linear 4s infinite}
@keyframes spin{to{transform:rotateY(360deg)}}
.petal{position:absolute;left:50%;bottom:0;width:var(--w);height:var(--h);margin-left:calc(var(--w) / -2);transform-origin:50% 100%;opacity:0;
  border-radius:48% 48% 28% 28% / 62% 62% 30% 30%;filter:brightness(var(--c));
  background:radial-gradient(ellipse at 50% 90%,rgba(255,130,150,.5),transparent 55%),linear-gradient(to top,#4a000c,#9d0f27 45%,#d62840);
  box-shadow:inset 0 -12px 20px rgba(0,0,0,.4),inset 0 7px 12px rgba(255,255,255,.14);
  transform:rotateY(var(--a)) translateZ(calc(var(--r) * .25)) rotateX(var(--ct)) scale(.45);
  transition:transform 2.4s cubic-bezier(.2,.85,.25,1) var(--d),opacity 1s ease var(--d)}
.bloom .petal{opacity:1;transform:rotateY(var(--a)) translateZ(var(--r)) rotateX(var(--ot)) scale(1)}
.glow{position:absolute;left:-90px;top:-150px;width:180px;height:180px;border-radius:50%;background:radial-gradient(circle,rgba(255,90,115,.4),transparent 65%);opacity:0;transition:opacity 2s 1.5s;pointer-events:none;transform:translateZ(-60px)}
.bloom .glow{opacity:1}

/* falling petals */
.fp{position:fixed;top:-30px;border-radius:60% 0 60% 0;background:linear-gradient(135deg,#ff5d78,#a30f2a);opacity:.85;pointer-events:none;z-index:3;animation:fall linear forwards}
@keyframes fall{to{transform:translate3d(var(--dx),110vh,0) rotate(var(--rot));opacity:.3}}

/* final text */
.end{position:fixed;left:0;right:0;bottom:max(5vh,calc(env(safe-area-inset-bottom,0px) + 18px));text-align:center;padding:0 22px;z-index:10;pointer-events:none}
.line{width:0;height:1px;margin:0 auto 18px;background:linear-gradient(90deg,transparent,var(--gold),transparent);transition:width 1.6s ease}
.show .line{width:min(220px,50vw)}
.tag{font-family:var(--serif);font-style:italic;font-weight:400;font-size:clamp(18px,3.4vw,30px);line-height:1.45;max-width:760px;margin:0 auto;
  color:#fff3e6;text-shadow:0 0 18px rgba(255,120,145,.55),0 2px 24px rgba(230,57,80,.5)}
.tag span{opacity:0;display:inline-block;animation:ch .7s ease forwards;animation-play-state:paused}
.show .tag span{animation-play-state:running}
@keyframes ch{from{opacity:0;transform:translateY(8px);filter:blur(5px)}to{opacity:1;transform:none;filter:none}}
.replay{pointer-events:auto;margin-top:18px;padding:9px 20px;border-radius:999px;border:1px solid var(--line);background:var(--glass);color:var(--muted);
  font:500 12.5px var(--sans);letter-spacing:.08em;cursor:pointer;opacity:0;transition:opacity 1s,color .2s,border-color .2s}
.show .replay{opacity:1;transition-delay:5.5s}
.replay:hover{color:var(--ink);border-color:var(--rose)}

@media(max-width:560px){.wrap{transform-origin:50% 0;scale:.78}}
@media(max-height:700px){.wrap{scale:.78;transform-origin:50% 0}}
@media(max-height:560px){.wrap{scale:.62}}
@media(prefers-reduced-motion:reduce){*{animation-duration:.01s!important;transition-duration:.01s!important;transition-delay:0s!important}}
</style>
</head>
<body>
<div class="vignette"></div>
<div class="spotlight"></div>
<div class="ambient"></div>
<div class="dust" id="dust"></div>

<div class="overlay" id="overlay">
  <div class="card">
    <div class="icon">🌹</div>
    <h2>Love.css</h2>
    <div class="bar"><i id="bar"></i></div>
    <p class="status" id="status">Preparing petals…</p>
    <button class="btn" id="go" disabled>Tap to Bloom</button>
  </div>
</div>

<div class="scene">
  <div class="wrap" id="wrap">
    <div class="stem"></div>
    <div class="thorn t1"></div><div class="thorn t2"></div>
    <div class="leaf ll"></div><div class="leaf lr"></div>
    <div class="calyx"></div>
    <div class="head"><div class="glow"></div><div class="spin" id="spin"></div></div>
  </div>
</div>

<div class="end" id="end">
  <div class="line"></div>
  <p class="tag" id="tag" aria-label="I love you so much farawlty, dalo3ty, nadody, my girl and my life"></p>
  <button class="replay" id="replay">↻ Replay</button>
</div>

<script>
(function(){
  var $=function(i){return document.getElementById(i)};
  var body=document.body,wrap=$('wrap'),spin=$('spin'),end=$('end');

  // petals: [count, radius, height, width, closedTilt, openTilt]
  var L=[[5,5,62,44,16,-2],[5,13,78,56,14,-18],[7,24,94,66,12,-38],[8,36,110,78,10,-57],[9,50,124,90,8,-78]];
  L.forEach(function(l,li){
    for(var i=0;i<l[0];i++){
      var p=document.createElement('div');p.className='petal';
      var s=p.style;
      s.setProperty('--a',(i*360/l[0]+li*27+Math.random()*10)+'deg');
      s.setProperty('--r',l[1]+'px');s.setProperty('--h',l[2]+'px');s.setProperty('--w',l[3]+'px');
      s.setProperty('--ct',l[4]+'deg');s.setProperty('--ot',l[5]+'deg');
      s.setProperty('--d',(li*.4+i*.06)+'s');
      s.setProperty('--c',(.78+li*.07+Math.random()*.08).toFixed(2));
      spin.appendChild(p);
    }
  });

  // typed tagline
  var txt='I love you so much farawlty, dalo3ty, nadody, my girl and my life';
  var tag=$('tag');
  var n=0;
  txt.split(' ').forEach(function(w,wi,arr){
    var word=document.createElement('span');word.style.cssText='display:inline-block;opacity:1;animation:none';
    w.split('').forEach(function(c,ci){
      var s=document.createElement('span');s.textContent=c;
      s.style.animationDelay=(4.2+(n++)*.055)+'s';
      word.appendChild(s);
    });
    tag.appendChild(word);
    if(wi<arr.length-1)tag.appendChild(document.createTextNode(' '));
  });

  // ambient dust
  var dust=$('dust');
  for(var d=0;d<26;d++){var i=document.createElement('i');
    i.style.cssText='left:'+Math.random()*100+'%;animation-duration:'+(10+Math.random()*14)+'s;animation-delay:'+(-Math.random()*20)+'s;--dx:'+(Math.random()*80-40)+'px;width:'+(2+Math.random()*3)+'px;height:'+(2+Math.random()*3)+'px';
    dust.appendChild(i);}

  // loader
  var bar=$('bar'),status=$('status'),go=$('go'),pct=0;
  var msgs=['Preparing petals…','Compiling thorns…','Rendering fragrance…','Ready to bloom'];
  var t=setInterval(function(){
    pct=Math.min(100,pct+4+Math.random()*7);
    bar.style.width=pct+'%';
    status.textContent=msgs[Math.min(3,Math.floor(pct/34))];
    if(pct>=100){clearInterval(t);go.disabled=false;}
  },110);

  // falling petals
  var fall=null;
  function drop(){
    var f=document.createElement('div');f.className='fp';
    var sz=10+Math.random()*12;
    f.style.cssText='left:'+Math.random()*100+'vw;width:'+sz+'px;height:'+sz*1.3+'px;animation-duration:'+(7+Math.random()*6)+'s;--dx:'+(Math.random()*240-120)+'px;--rot:'+(Math.random()*720-360)+'deg';
    document.body.appendChild(f);
    f.addEventListener('animationend',function(){f.remove()});
  }

  function start(){
    $('overlay').classList.add('hide');
    setTimeout(function(){wrap.classList.add('grow')},500);
    setTimeout(function(){wrap.classList.add('bloom');body.classList.add('bloomed')},2100);
    setTimeout(function(){end.classList.add('show');fall=setInterval(drop,650)},4200);
  }
  go.addEventListener('click',start);

  $('replay').addEventListener('click',function(){
    clearInterval(fall);
    end.classList.remove('show');wrap.classList.remove('bloom','grow');body.classList.remove('bloomed');
    document.querySelectorAll('.fp').forEach(function(n){n.remove()});
    void wrap.offsetWidth;setTimeout(start,400);
  });

  // subtle parallax
  window.addEventListener('mousemove',function(e){
    var x=(e.clientX/innerWidth-.5),y=(e.clientY/innerHeight-.5);
    wrap.style.transform='rotateY('+x*10+'deg) rotateX('+(-y*4)+'deg)';
  });
})();
</script>
</body>
</html>
