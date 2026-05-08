<div align="center">

<h2 class="sr-only">Animated pixel art display of the name Aditya Hawaldar</h2>
<style>
*{box-sizing:border-box;margin:0;padding:0}
#c{display:block;background:#0D1117;width:100%;border-radius:12px}
.sub{text-align:center;font-size:13px;color:#378ADD;letter-spacing:0.15em;padding:10px 0 14px;font-family:monospace}
</style>
<canvas id="c" height="180"></canvas>
<div class="sub">AI ENGINEER &nbsp;·&nbsp; FINTECH BUILDER &nbsp;·&nbsp; GCE KOLHAPUR</div>
<script>
const C=document.getElementById('c');
const ctx=C.getContext('2d');
const W=C.offsetWidth||680;
C.width=W;

const FONT=[
  {ch:'A',w:7,rows:["_XXX___","X___X__","X___X__","XXXXX__","X___X__","X___X__","X___X__"]},
  {ch:'D',w:7,rows:["XXXX___","X___X__","X____X_","X____X_","X____X_","X___X__","XXXX___"]},
  {ch:'I',w:5,rows:["XXXXX_","__X___","__X___","__X___","__X___","__X___","XXXXX_"]},
  {ch:'T',w:7,rows:["XXXXX__","__X____","__X____","__X____","__X____","__X____","__X____"]},
  {ch:'Y',w:7,rows:["X___X__","X___X__","_X_X___","__X____","__X____","__X____","__X____"]},
  {ch:'A',w:7,rows:["_XXX___","X___X__","X___X__","XXXXX__","X___X__","X___X__","X___X__"]},
  {ch:' ',w:4,rows:["____","____","____","____","____","____","____"]},
  {ch:'H',w:7,rows:["X___X__","X___X__","X___X__","XXXXX__","X___X__","X___X__","X___X__"]},
  {ch:'A',w:7,rows:["_XXX___","X___X__","X___X__","XXXXX__","X___X__","X___X__","X___X__"]},
  {ch:'W',w:9,rows:["X_____X__","X_____X__","X__X__X__","X_X_X_X__","X_X_X_X__","_X___X___","_X___X___"]},
  {ch:'A',w:7,rows:["_XXX___","X___X__","X___X__","XXXXX__","X___X__","X___X__","X___X__"]},
  {ch:'L',w:6,rows:["X_____","X_____","X_____","X_____","X_____","X_____","XXXXX_"]},
  {ch:'D',w:7,rows:["XXXX___","X___X__","X____X_","X____X_","X____X_","X___X__","XXXX___"]},
  {ch:'A',w:7,rows:["_XXX___","X___X__","X___X__","XXXXX__","X___X__","X___X__","X___X__"]},
  {ch:'R',w:7,rows:["XXXX___","X___X__","X___X__","XXXX___","X_X____","X__X___","X___X__"]},
];

const PS=6;
const GAP=2;
const ROWS=7;

let cols=[];
let xOff=0;
FONT.forEach(f=>{
  const w=f.rows[0].length;
  for(let c=0;c<w;c++){
    let col=[];
    for(let r=0;r<ROWS;r++) col.push(f.rows[r][c]==='X'?1:0);
    cols.push({pixels:col,x:xOff});
    xOff+=(PS+GAP);
  }
  xOff+=GAP;
});

const totalW=xOff;
const totalH=ROWS*(PS+GAP);
const startX=(W-totalW)/2;
const startY=(180-totalH)/2;

const BLUE='#2E6DB4';
const BRIGHT='#85B7EB';
const DIM='#0C447C';
const SPARK='#B5D4F4';

let particles=[];
let t=0;
let phase='build';
let buildIdx=0;
let buildTimer=0;
let revealed=new Array(cols.length).fill(false);

function spawnSpark(x,y){
  for(let i=0;i<3;i++){
    particles.push({
      x:x+PS/2,y:y+PS/2,
      vx:(Math.random()-0.5)*3,
      vy:(Math.random()-0.5)*3,
      life:1,decay:0.05+Math.random()*0.05,
      size:Math.random()*2+1
    });
  }
}

function draw(){
  ctx.clearRect(0,0,W,180);
  ctx.fillStyle='#0D1117';
  ctx.fillRect(0,0,W,180);

  t+=0.03;

  if(phase==='build'){
    buildTimer++;
    if(buildTimer>1 && buildIdx<cols.length){
      revealed[buildIdx]=true;
      const col=cols[buildIdx];
      for(let r=0;r<ROWS;r++){
        if(col.pixels[r]){
          const px=startX+col.x;
          const py=startY+r*(PS+GAP);
          spawnSpark(px,py);
        }
      }
      buildIdx++;
      buildTimer=0;
      if(buildIdx>=cols.length) phase='idle';
    }
  }

  cols.forEach((col,ci)=>{
    if(!revealed[ci]) return;
    for(let r=0;r<ROWS;r++){
      const px=startX+col.x;
      const py=startY+r*(PS+GAP);
      if(col.pixels[r]){
        const wave=Math.sin(t+ci*0.18+r*0.25);
        const brightness=0.7+0.3*wave;
        if(brightness>0.85){
          ctx.fillStyle=BRIGHT;
        } else if(brightness>0.7){
          ctx.fillStyle=BLUE;
        } else {
          ctx.fillStyle=DIM;
        }
        ctx.fillRect(px,py,PS,PS);
      } else {
        ctx.fillStyle='#0D1117';
        ctx.fillRect(px,py,PS,PS);
      }
    }
  });

  particles=particles.filter(p=>p.life>0);
  particles.forEach(p=>{
    ctx.globalAlpha=p.life;
    ctx.fillStyle=SPARK;
    ctx.fillRect(p.x,p.y,p.size,p.size);
    p.x+=p.vx;
    p.y+=p.vy;
    p.life-=p.decay;
    p.vx*=0.92;
    p.vy*=0.92;
  });
  ctx.globalAlpha=1;

  if(phase==='idle'){
    const scanX=((t*30)%(totalW+80))-40;
    const gx=startX+scanX;
    ctx.fillStyle='rgba(53,138,221,0.08)';
    ctx.fillRect(gx,startY-4,12,totalH+8);
  }

  requestAnimationFrame(draw);
}

draw();
</script>

<p>
  <a href="https://www.linkedin.com/in/aditya-havaldar-205951288/">
    <img src="https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white"/>
  </a>
  <a href="mailto:adityahavaldar07@gmail.com">
    <img src="https://img.shields.io/badge/Gmail-EA4335?style=for-the-badge&logo=gmail&logoColor=white"/>
  </a>
  <a href="https://github.com/Aditya0292">
    <img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white"/>
  </a>
</p>

<img src="https://komarev.com/ghpvc/?username=Aditya0292&style=flat-square&color=1A3C6E&label=Profile+Views"/>

</div>

---

## 🧠 About Me

```python
class AdityaHawaldar:
    def __init__(self):
        self.name        = "Aditya Amit Hawaldar"
        self.role        = "AI & Data Science Engineer"
        self.university  = "Government College of Engineering, Kolhapur"
        self.year        = "3rd Year B.Tech (2023–2027)"
        self.location    = "Kolhapur, India"

        self.focus       = ["Algorithmic Trading", "LLM Applications", "FinTech AI"]
        self.stack       = ["Python", "Next.js", "TensorFlow", "Web3.py"]
        self.currently   = "Building production-grade AI systems & contributing to open source"
        self.goal        = "AI Engineer at a FinTech or AI-first company"

    def say_hi(self):
        print("I don't just learn AI — I build with it.")
```

---

## 🚀 Featured Projects

<table>
<tr>
<td width="50%" valign="top">

### ⚡ APEX Trade AI
**Institutional Intelligence OS**

> Autonomous algorithmic trading system with a 4-model ML ensemble + blockchain-verified signal ledger.

![Python](https://img.shields.io/badge/Python-3670A0?style=flat-square&logo=python&logoColor=white)
![TensorFlow](https://img.shields.io/badge/TensorFlow-FF6F00?style=flat-square&logo=tensorflow&logoColor=white)
![Next.js](https://img.shields.io/badge/Next.js-000?style=flat-square&logo=next.js)
![Polygon](https://img.shields.io/badge/Polygon-8247E5?style=flat-square&logo=polygon&logoColor=white)

- 🤖 4-model ensemble: XGBoost + LightGBM + BiLSTM + Transformer
- ⛓️ SHA-256 signal hashing anchored on Polygon Amoy blockchain
- 📊 Half-Kelly risk engine + live Next.js institutional dashboard
- 🔁 Runs 24/7 as autonomous Windows service

</td>
<td width="50%" valign="top">

### 🌊 NeuroFlow
**AI-Powered Learning OS**

> Gamified ML curriculum platform with live sandboxed code execution and Gemini AI tutoring.

![Next.js](https://img.shields.io/badge/Next.js_14-000?style=flat-square&logo=next.js)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)
![Gemini](https://img.shields.io/badge/Gemini_2.0_Flash-4285F4?style=flat-square&logo=google&logoColor=white)
![Tailwind](https://img.shields.io/badge/Tailwind-06B6D4?style=flat-square&logo=tailwindcss&logoColor=white)

- ⚡ Real-time algorithmic visualizers for ML concepts
- 🐍 Sandboxed Python execution via Piston API (zero setup)
- 🤖 Gemini 2.0 Flash as context-aware AI tutor
- 🎨 Custom "Liquid Glass" design system

</td>
</tr>
<tr>
<td width="50%" valign="top">

### 🧬 MetaLearn AI
**Adaptive Learning PWA**

> Full-stack AI-powered Progressive Web App with LLM-driven personalized learning flows.

![React](https://img.shields.io/badge/React-20232A?style=flat-square&logo=react&logoColor=61DAFB)
![Supabase](https://img.shields.io/badge/Supabase-3ECF8E?style=flat-square&logo=supabase&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=flat-square&logo=node.js&logoColor=white)

- 🔐 Supabase auth, profiles & real-time session management
- 🧠 LLM-driven adaptive learning logic
- 📱 Cross-platform PWA (desktop + mobile)

</td>
<td width="50%" valign="top">

### 📊 AI Feedback Analysis System
**NLP-Powered Analytics Engine**

> Transformer-based pipeline for automated sentiment analysis and instructor reporting.

![Python](https://img.shields.io/badge/Python-3670A0?style=flat-square&logo=python&logoColor=white)
![Transformers](https://img.shields.io/badge/Transformers-FFD21E?style=flat-square&logo=huggingface&logoColor=black)
![Express](https://img.shields.io/badge/Express-000?style=flat-square&logo=express)

- 🔍 Multi-class sentiment + topic classification on 500+ entries
- 📉 Reduced manual processing time by **80%**
- 📋 Auto-generated instructor-wise analytical reports

</td>
</tr>
</table>

---

## 🛠️ Tech Stack

**AI & Data Science**

![Python](https://img.shields.io/badge/Python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)
![TensorFlow](https://img.shields.io/badge/TensorFlow-FF6F00?style=for-the-badge&logo=tensorflow&logoColor=white)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white)

**Frontend**

![Next.js](https://img.shields.io/badge/Next.js-000000?style=for-the-badge&logo=next.js&logoColor=white)
![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white)

**Backend & Database**

![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=node.js&logoColor=white)
![Express](https://img.shields.io/badge/Express-000000?style=for-the-badge&logo=express&logoColor=white)
![Supabase](https://img.shields.io/badge/Supabase-3ECF8E?style=for-the-badge&logo=supabase&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-4EA94B?style=for-the-badge&logo=mongodb&logoColor=white)

**Web3 & Tools**

![Web3.py](https://img.shields.io/badge/Web3.py-F16822?style=for-the-badge&logo=web3dotjs&logoColor=white)
![Polygon](https://img.shields.io/badge/Polygon-8247E5?style=for-the-badge&logo=polygon&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05033?style=for-the-badge&logo=git&logoColor=white)
![Vercel](https://img.shields.io/badge/Vercel-000000?style=for-the-badge&logo=vercel&logoColor=white)

---


## 📈 GitHub Stats

<div align="center">
  <img height="160" src="https://github-readme-stats.vercel.app/api?username=Aditya0292&show_icons=true&theme=tokyonight&hide_border=true&bg_color=0D1117&title_color=2E6DB4&icon_color=1A3C6E&text_color=c9d1d9"/>
  <img height="160" src="https://github-readme-stats.vercel.app/api/top-langs/?username=Aditya0292&layout=compact&theme=tokyonight&hide_border=true&bg_color=0D1117&title_color=2E6DB4&text_color=c9d1d9"/>
</div>

<div align="center">
  <img src="https://nirzak-streak-stats.vercel.app/?user=Aditya0292&theme=tokyonight&hide_border=true&background=0D1117&ring=2E6DB4&fire=1A3C6E&currStreakLabel=2E6DB4"/>
</div>

---

## 🌱 Currently

- 🔭 Building: **APEX Trade AI v2** — adding reinforcement learning layer
- 🌱 Learning: **LangGraph agents** + **OpenBB** open source contributions  
- 👯 Open to: **Internships** in AI/ML, FinTech, Full-Stack
- 💬 Ask me about: **Algorithmic trading**, **LLM integration**, **Next.js**
- 📫 Reach me: **adityahavaldar07@gmail.com**

---

<div align="center">

*"I don't just study AI — I ship it."*

<img src="https://capsule-render.vercel.app/api?type=waving&color=1A3C6E&height=100&section=footer"/>

</div>
