# vanelollixiom.github.io
let questions=[];
let responses=[];
let current=0;
let screens={};
let canvas;
let qNum,qAxis,qText,progressBar,choices,backButton;
let resultTitle,resultName,resultSub;
let chart1,chart2,chart3;
let chartWrap1,chartWrap2,chartWrap3;
let scores;
let definitionsGrid;
let mySound=null;
let soundLoaded=false;
let musicAudio=null;
let musicMuted=false;
let musicStarted=false;
let musicToggle=null;

const MUSIC_URL="https://cdn.jsdelivr.net/gh/vanelollixiom/Kalama-Siwala@main/mikanzil%2C%20PSYQUI%20-%20Pallet%20%28Instrumental%29.mp3";
const RAW_MUSIC_URL="https://raw.githubusercontent.com/vanelollixiom/Kalama-Siwala/main/mikanzil%2C%20PSYQUI%20-%20Pallet%20%28Instrumental%29.mp3";

const CHART_IMAGE="https://i.pinimg.com/736x/5b/d8/68/5bd868dff8c71b5ac49ee26201393cb4.jpg";

const QUESTIONS=[
  ["aisthecel","When I think about information, it often comes with an associated visual, auditory, tactile, spatial, or other sensory quality."],
  ["amodalcel","I can process information without experiencing it through any particular sensory modality."],
  ["aisthecel","Imagining an object often involves some kind of internally perceived sensory representation of that object."],
  ["amodalcel","I can reason about something while its representation remains completely detached from imagined sensory experience."],
  ["aisthecel","Different kinds of information tend to acquire different sensory qualities in my thinking."],
  ["amodalcel","Sensory qualities can disappear from my thinking without interfering with my ability to manipulate the information itself."],
  ["aisthecel","When recalling information, I often remember how it looked, sounded, felt, or otherwise presented itself perceptually."],
  ["amodalcel","The informational structure of a thought is usually more important to my processing than any sensory form it might take."],

  ["situcel","I naturally think about things in terms of where they stand relative to other things."],
  ["atopocel","I can understand a system without needing to mentally place its elements in relation to one another."],
  ["situcel","Moving one element within a mental model can change how I understand the other elements around it."],
  ["atopocel","The specific position of something in my mental model often feels irrelevant to its overall meaning."],
  ["situcel","When imagining a situation, I tend to construct relationships such as above, below, beside, inside, outside, near, or far."],
  ["atopocel","I tend to perceive an entire conceptual situation as a generalized configuration rather than as distinct positions occupied by individual elements."],
  ["situcel","I can mentally track how changing one thing's position affects the structure surrounding it."],
  ["atopocel","Several conceptually different things can occupy roughly the same mental \"place\" for me without needing to be spatially distinguished."],

  ["enargcel","Some of my thoughts feel like mental objects that I could almost reach out and interact with."],
  ["evanescel","My thoughts often feel more like fleeting traces than internally substantial objects."],
  ["enargcel","When I concentrate on an idea, it can acquire a distinct presence or solidity in my mind."],
  ["evanescel","A thought can strongly affect my reasoning while remaining extremely faint or insubstantial in my awareness."],
  ["enargcel","I can mentally inspect certain thoughts as though they possess a stable internal structure."],
  ["evanescel","My thoughts frequently disappear or transform before I can experience them as stable mental phenomena."],
  ["enargcel","Some concepts have an almost tangible cognitive presence even when I cannot explain exactly why."],
  ["evanescel","I often work with thoughts whose existence feels more like an elusive implication than a definite mental object."],

  ["lexicel","When I think about something, I tend to determine what it means before I can meaningfully manipulate the thought."],
  ["asemcel","I can mentally work with an idea even when I have no clear sense of what the idea means."],
  ["lexicel","Changing the meaning I assign to something can substantially change how I think about it."],
  ["asemcel","Some of my thoughts seem to operate effectively without acquiring any particular semantic interpretation."],
  ["lexicel","When an idea feels ambiguous, I tend to resolve the ambiguity by distinguishing between different possible meanings."],
  ["asemcel","I often manipulate thoughts whose internal content remains difficult to characterize, even to myself."],
  ["lexicel","I naturally notice semantic distinctions between thoughts that initially seem equivalent."],
  ["asemcel","A thought can remain cognitively useful to me while feeling almost completely devoid of identifiable meaning."],

  ["abstractcel","I experience raw observations instantly converting into a network of pure symbolic relationships."],
  ["abstractcel","Individual specifics hold less weight for me than the relational logic connecting them."],
  ["abstractcel","I register events primarily as localized expressions of an underlying, invariant rule."],
  ["abstractcel","The immediate friction of an experience vanishes as soon as I extract its core concept."],

  ["ephemeralcel","I experience sensory inputs in their pure state before interpretation takes over."],
  ["ephemeralcel","The moment an experience ends, any mental structure built around it instantly vanishes."],
  ["ephemeralcel","My immediate perceptions remain unclouded by stored biases or internal cognitive overlays."],
  ["ephemeralcel","My mental representations exist only for as long as an active stimulus is directly present to sustain them."],

  ["salientcel","Ambiguous situations instantly crystallize in my mind around their most striking feature."],
  ["salientcel","The most intense details of an environment immediately dictate my mental focus."],
  ["salientcel","I rapidly reframe chaotic impressions into a clean, decisive focal point."],
  ["salientcel","Subtle background noise fades quickly as I zero in on the central, high-impact display."],

  ["obscurecel","My attention naturally scans peripheral anomalies rather than natural conceptualizations."],
  ["obscurecel","I easily translate ideas across marginal systems of thought that seem mutually incompatible."],
  ["obscurecel","My awareness remains wide-angled and decentralized rather than zeroing in on one point."],
  ["obscurecel","I am drawn to subtle, non-standard details that lie completely outside regular frameworks."]
];

const DEFINITIONS={
  asemcel:"Thinker manipulates thought through a raw implicit void-likeness.",
  lexicel:"Thinker that manipulates thought through semantic attribution.",
  aisthecel:"Thinker that manifests sensory modalities attached to their thoughts (Sensation-based interface for information).",
  amodalcel:"Thinker devoided of sensory modalities in their thought space (sheer informational processing).",
  enargcel:"Thinker that presents tangible-adjacent thought phenomena (high opacity thoughts).",
  evanescel:"Thinker with wraith-like thought phenomena (low opacity thoughts).",
  atopocel:"Thinker that presents a superpositioned positionally indistinct/generalized thought ecosystem.",
  situcel:"Thinker that manifests positional relations in their thought ecosystem.",

  abstractcel:"Cognition that transmutes into noetic virtualization its raw perceptual data, synchronizin' with the de-individuated symbolic matrix through the complete sublimation of immediate experiential friction into absolute propositional topologies and the automatic generation of pure conceptual invariants that supersede all localized empirical variance",

  ephemeralcel:"Cognition that partakes in unblemished fidelity its pristine perceptual models, unsynchronizin' from psychic taint via the absolute safeguarding of primordial sensory signatures prior to systemic alteration and the instantaneous volatilization of structural constructs upon cessation of active stimuli",

  salientcel:"Cognition that locks onto the searing impact of frontline ostensions, unsynchronizin' from cryptic fringe configurations through complete harmonization with dominant phenomenal displays across environmental gradients, coupled with agile clarification and rapid structural reframin' that instantly crystallizes ambiguous inputs into dominant focal nodes",

  obscurecel:"Cognition that vinculates to esoteric anomaly tokens and recondite occultations, unsynchronizin' from overt signals through wide-angle scanning across disparate non-canonical domains without centralized focal points, alongside fluid conversion protocols operatin' seamlessly across incompatible marginal sign systems."
};

const ANSWERS=[
  "Strongly disagree",
  "Disagree",
  "Neutral",
  "Agree",
  "Strongly agree"
];

const CHART1_ZONES=[
  ["Âmšta","Aškła","Ižpřal","Otǫkřal"],
  ["Âšřal","Il’žřan","A†kal","Ukšmâl"],
  ["Âžmłat","Ištral","Otřal","Azkřal"],
  ["Âžłan","Il’krat","Ezřkal","Uşkal"]
];

const CHART2_ZONES=[
  ["Otǫkšal","Ezprân","Ušpral’","Ašvlaâ"],
  ["Ukšmâ†","A†krân","Aǫžroł","Âšřim"],
  ["Izmraṭ","Očpral","Ežmłoš","Uşkan"],
  ["Âžkran","Ičpruṭ","Aǫprâš","Âžvla"]
];

const CHART3_ZONES=[
  ["mipsu'u","mipsidbo","miple'i","mipvītci"],
  ["tolclasi'o","nalcma","nalflecu","tolvitci"],
  ["jarsidbo","jarcma","jarflecu","jarvitci"],
  ["visrasid","cladjo'i","vismu'e","visravitci"]
];

function preload(){
  try{
    mySound=loadSound(
      MUSIC_URL,
      ()=>{
        soundLoaded=true;

        if(musicStarted&&mySound&&!mySound.isPlaying()){
          stopNativeMusic();

          try{
            userStartAudio();
          }catch(e){}

          mySound.setVolume(
            musicMuted?0:1
          );

          mySound.loop();
        }
      },
      error=>{
        soundLoaded=false;

        console.warn(
          "p5.sound could not load the MP3. Using native audio fallback.",
          error
        );
      }
    );
  }catch(error){
    soundLoaded=false;

    console.warn(
      "p5.sound is unavailable. Using native audio fallback.",
      error
    );
  }
}

function setup(){
  pixelDensity(1);

  canvas=createCanvas(
    windowWidth,
    windowHeight
  );

  canvas.position(0,0);

  canvas.style(
    "position",
    "fixed"
  );

  canvas.style(
    "z-index",
    "0"
  );

  canvas.style(
    "top",
    "0"
  );

  canvas.style(
    "left",
    "0"
  );

  canvas.style(
    "pointer-events",
    "none"
  );

  document.body.style.margin="0";
  document.body.style.overflowX="hidden";
  document.body.style.fontFamily="Arial, Helvetica, sans-serif";
  document.body.style.color="#eef7f3";
  document.body.style.background="transparent";
  document.body.style.webkitTapHighlightColor="transparent";

  document.documentElement.style.background="#071014";
  document.documentElement.style.webkitTextSizeAdjust="100%";
  document.documentElement.style.textSizeAdjust="100%";

  addStyles();
  createInterface();

  const keyboardFocus=
    document.createElement("button");

  keyboardFocus.type="button";

  keyboardFocus.setAttribute(
    "aria-hidden",
    "true"
  );

  keyboardFocus.tabIndex=0;

  keyboardFocus.style.position="fixed";
  keyboardFocus.style.left="-9999px";
  keyboardFocus.style.top="-9999px";
  keyboardFocus.style.width="1px";
  keyboardFocus.style.height="1px";
  keyboardFocus.style.opacity="0";
  keyboardFocus.style.pointerEvents="none";
  keyboardFocus.style.border="0";
  keyboardFocus.style.padding="0";

  document.body.appendChild(
    keyboardFocus
  );

  try{
    keyboardFocus.focus({
      preventScroll:true
    });
  }catch(e){
    try{
      keyboardFocus.focus();
    }catch(ignored){}
  }

  const handleKeyboard=event=>{

    if(
      (
        event.key==="Enter"||
        event.code==="Enter"||
        event.keyCode===13
      )&&
      screens.intro&&
      screens.intro.elt.style.display!=="none"
    ){
      event.preventDefault();
      event.stopPropagation();

      startMusic();
      startTest();

      return;
    }

    if(
      screens.question&&
      screens.question.elt.style.display!=="none"
    ){

      let answerIndex=-1;

      if(
        event.code==="Digit1"||
        event.code==="Numpad1"||
        event.key==="1"||
        event.keyCode===49||
        event.keyCode===97
      ){
        answerIndex=0;

      }else if(
        event.code==="Digit2"||
        event.code==="Numpad2"||
        event.key==="2"||
        event.keyCode===50||
        event.keyCode===98
      ){
        answerIndex=1;

      }else if(
        event.code==="Digit3"||
        event.code==="Numpad3"||
        event.key==="3"||
        event.keyCode===51||
        event.keyCode===99
      ){
        answerIndex=2;

      }else if(
        event.code==="Digit4"||
        event.code==="Numpad4"||
        event.key==="4"||
        event.keyCode===52||
        event.keyCode===100
      ){
        answerIndex=3;

      }else if(
        event.code==="Digit5"||
        event.code==="Numpad5"||
        event.key==="5"||
        event.keyCode===53||
        event.keyCode===101
      ){
        answerIndex=4;
      }

      if(answerIndex!==-1){

        event.preventDefault();
        event.stopPropagation();

        selectAnswer(
          answerIndex
        );
      }
    }
  };

  window.addEventListener(
    "keydown",
    handleKeyboard,
    true
  );

  document.addEventListener(
    "keydown",
    handleKeyboard,
    true
  );

  showScreen("intro");
  drawBackground();
  noLoop();
}

function windowResized(){

  resizeCanvas(
    windowWidth,
    windowHeight
  );

  drawBackground();
  positionElements();
}

function drawBackground(){

  background("#071014");
  noStroke();

  const mobile=
    width<=780;

  const radius=
    mobile?220:340;

  const spacing=
    mobile?10:7;

  function glow(
    cx,
    cy,
    rgb,
    strength
  ){

    for(
      let r=0;
      r<radius;
      r+=spacing
    ){

      const t=
        r/radius;

      fill(
        rgb[0],
        rgb[1],
        rgb[2],
        lerp(
          strength,
          0,
          t
        )
      );

      ellipse(
        cx,
        cy,
        r*3.4,
        r*3.4
      );
    }
  }

  glow(
    width*.02,
    height*.02,
    [38,95,210],
    18
  );

  glow(
    width*.98,
    height*.02,
    [61,220,122],
    18
  );

  glow(
    width*.02,
    height*.98,
    [230,0,182],
    20
  );

  glow(
    width*.98,
    height*.98,
    [241,225,29],
    18
  );

  stroke(
    255,
    255,
    255,
    13
  );

  strokeWeight(1);

  const grid=
    mobile?42:36;

  for(
    let x=0;
    x<=width;
    x+=grid
  ){

    line(
      x,
      0,
      x,
      height
    );
  }

  for(
    let y=0;
    y<=height;
    y+=grid
  ){

    line(
      0,
      y,
      width,
      y
    );
  }
}

function addStyles(){

  const style=
    document.createElement("style");

  style.innerHTML=`

  *{
    box-sizing:border-box
  }

  button{
    font-family:
      Arial,
      Helvetica,
      sans-serif
  }

  .screen{
    position:relative;
    z-index:1;
    width:100%;
    min-height:100vh;
    padding:26px 14px
  }

  .card{
    width:min(1180px,96vw);
    margin:0 auto;
    padding:30px 34px 34px;

    border:
      1px solid rgba(255,255,255,.16);

    border-radius:28px;

    background:
      linear-gradient(
        140deg,
        rgba(11,20,25,.90),
        rgba(13,28,29,.84) 48%,
        rgba(15,23,18,.88)
      ),
      radial-gradient(
        circle at 20% 10%,
        rgba(72,170,255,.12),
        transparent 35%
      ),
      radial-gradient(
        circle at 85% 85%,
        rgba(255,196,0,.10),
        transparent 38%
      );

    box-shadow:
      0 30px 90px rgba(0,0,0,.45),
      0 0 0 1px rgba(95,235,183,.05),
      inset 0 0 60px rgba(255,255,255,.02);

    backdrop-filter:blur(10px)
  }

  .intro-card{
    min-height:
      calc(100vh - 52px);

    display:flex;
    flex-direction:column;
    justify-content:center
  }

  .eyebrow{
    text-align:center;
    font-size:11px;
    font-weight:900;
    letter-spacing:.26em;
    text-transform:uppercase;
    color:#7ddfe0
  }

  .intro-brand{
    margin-top:8px;
    text-align:center;

    font:
      900 clamp(38px,7vw,78px)/.92
      Impact,
      Haettenschweiler,
      "Arial Black",
      sans-serif;

    letter-spacing:.03em;
    text-transform:uppercase;

    background:
      linear-gradient(
        90deg,
        #4d8dff,
        #2bd5c0,
        #67ee63,
        #f6e631,
        #ff5bc7,
        #4d8dff
      );

    -webkit-background-clip:text;
    background-clip:text;
    color:transparent
  }

  .intro-subtitle{
    max-width:900px;
    margin:16px auto 0;
    text-align:center;

    font:
      900 clamp(12px,1.7vw,15px)/1.5
      Arial,
      Helvetica,
      sans-serif;

    letter-spacing:.08em;
    text-transform:uppercase;
    color:#b8d9d8
  }

  .axis-pills{
    display:flex;
    flex-wrap:wrap;
    justify-content:center;
    gap:10px;

    margin:22px auto;

    max-width:950px
  }

  .axis-group{
    padding:10px 16px;
    border-radius:18px;

    border:
      1px solid rgba(255,255,255,.15);

    text-align:center;
    font-size:12px;
    font-weight:900;
    line-height:1.4;

    box-shadow:
      0 8px 18px rgba(0,0,0,.20)
  }

  .axis-group small{
    font-size:10px;
    opacity:.85
  }

  .group-1{
    background:
      linear-gradient(
        135deg,
        rgba(82,130,255,.30),
        rgba(70,232,165,.22)
      );

    color:#9de7ff
  }

  .group-2{
    background:
      linear-gradient(
        135deg,
        rgba(255,101,196,.24),
        rgba(115,116,255,.25)
      );

    color:#ffc8ed
  }

  .group-3{
    background:
      linear-gradient(
        135deg,
        rgba(255,164,72,.24),
        rgba(180,112,255,.25)
      );

    color:#ffd1a6
  }

  .start-button,
  .action-button{
    border:
      1px solid rgba(255,255,255,.20);

    border-radius:999px;
    padding:14px 24px;

    font-weight:900;
    font-size:15px;
    letter-spacing:.05em;

    color:#061015;

    cursor:pointer;
    touch-action:manipulation;

    transition:
      transform .16s ease,
      box-shadow .16s ease
  }

  .start-button:hover,
  .action-button:hover{
    transform:translateY(-2px)
  }

  .start-button{
    align-self:center;
    margin-top:10px;

    background:
      linear-gradient(
        90deg,
        #6f91ff,
        #32d3bd,
        #8ae64e,
        #f5dc32
      );

    box-shadow:
      0 16px 35px rgba(0,0,0,.30)
  }

  .keyboard-help{
    margin-top:12px;
    text-align:center;
    color:#7b9a9c;
    font-size:12px
  }

  .top-line{
    display:flex;
    justify-content:space-between;
    gap:12px;
    align-items:center
  }

  .badge{
    padding:8px 12px;
    border-radius:999px;

    border:
      1px solid rgba(255,255,255,.14);

    background:
      rgba(255,255,255,.05);

    color:#bfe5e3;

    font-weight:900;
    font-size:12px;
    letter-spacing:.06em;
    text-transform:uppercase
  }

  .progress{
    height:11px;
    margin:16px 0 25px;

    border-radius:999px;
    overflow:hidden;

    background:
      rgba(255,255,255,.06);

    border:
      1px solid rgba(255,255,255,.12)
  }

  .progress-bar{
    width:0;
    height:100%;
    border-radius:999px;

    background:
      linear-gradient(
        90deg,
        #4a84ff,
        #38d6c5,
        #84eb52,
        #f5d72a,
        #ff55c8
      );

    box-shadow:
      0 0 18px rgba(92,220,194,.28)
  }

  .q-text{
    max-width:940px;
    margin:0 auto 22px;

    text-align:center;

    font:
      700 clamp(24px,3.25vw,41px)/1.15
      Georgia,
      serif;

    color:#edf8f5;
    text-wrap:balance
  }

  .choices{
    display:grid;
    grid-template-columns:
      repeat(5,1fr);

    gap:12px;

    max-width:1000px;
    margin:0 auto
  }

  .choice{
    min-height:112px;

    padding:14px 10px;

    border-radius:18px;

    border:
      1px solid rgba(255,255,255,.12);

    background:
      linear-gradient(
        160deg,
        rgba(255,255,255,.07),
        rgba(255,255,255,.03)
      );

    color:#d9ebe9;

    font-weight:900;

    cursor:pointer;

    box-shadow:
      0 10px 25px rgba(0,0,0,.18);

    touch-action:manipulation;

    transition:
      transform .16s ease,
      border-color .16s ease,
      background .16s ease
  }

  .choice:hover{
    transform:translateY(-2px);

    border-color:
      rgba(132,235,82,.48)
  }

  .choice.selected{
    background:
      linear-gradient(
        135deg,
        #4d7dff,
        #38d5c5,
        #89ea56
      );

    color:#051014;

    border-color:
      rgba(255,255,255,.55)
  }

  .choice-number{
    font:
      900 25px/1
      Impact,
      "Arial Black",
      sans-serif;

    margin-bottom:9px
  }

  .choice-label{
    font-size:12px;
    line-height:1.25
  }

  .question-nav{
    display:flex;
    justify-content:space-between;
    align-items:center;

    gap:12px;
    margin-top:17px
  }

  .back-button{
    border:
      1px solid rgba(255,255,255,.14);

    background:
      rgba(255,255,255,.05);

    color:#b9d3d2;

    border-radius:999px;

    padding:9px 14px;

    font-weight:900;
    cursor:pointer
  }

  .question-help{
    color:#718f90;
    font-size:12px;
    text-align:center;
    flex:1
  }

  .result-title{
    text-align:center;

    font:
      900 clamp(27px,4.5vw,53px)/1
      "Arial Black",
      Impact,
      sans-serif;

    letter-spacing:.03em;
    text-transform:uppercase;

    color:#edf9f6
  }

  .result-name{
    margin-top:10px;
    text-align:center;

    font:
      900 clamp(23px,3.5vw,40px)/1.25
      "Arial Black",
      Impact,
      sans-serif;

    letter-spacing:.04em;
    text-transform:uppercase
  }

  .result-sub{
    margin:12px auto 20px;

    text-align:center;

    max-width:900px;

    font-size:15px;
    font-weight:900;
    line-height:1.55
  }

  .chart-section{
    margin-top:20px
  }

  .chart-heading{
    margin:28px 0 12px;

    text-align:center;

    font:
      900 clamp(21px,3vw,31px)/1
      "Arial Black",
      Impact,
      sans-serif;

    letter-spacing:.06em;
    text-transform:uppercase;

    color:#edf9f6
  }

  .chart-frame{
    display:grid;

    grid-template-columns:
      70px minmax(300px,760px) 70px;

    grid-template-rows:
      58px auto 58px;

    justify-content:center;
    align-items:center;

    width:100%
  }

  .axis-title{
    display:flex;

    align-items:center;
    justify-content:center;

    color:#effff8;

    font:
      900 clamp(14px,1.6vw,21px)/1
      "Arial Black",
      Impact,
      sans-serif;

    letter-spacing:.16em;
    text-transform:uppercase;

    text-shadow:
      0 0 14px rgba(255,255,255,.18)
  }

  .vertical-axis{
    writing-mode:vertical-rl
  }

  .vertical-axis.reverse{
    transform:rotate(180deg)
  }

  .chart-wrap{
    grid-column:2;
    grid-row:2;

    width:min(760px,100%);

    aspect-ratio:1/1;

    position:relative
  }

  .chart{
    position:absolute;
    inset:0;

    overflow:visible;

    border:none;
    box-shadow:none;
    background:transparent
  }

  .chart-image{
    position:absolute;
    inset:0;

    width:100%;
    height:100%;

    display:block;

    z-index:1;

    object-fit:fill;

    pointer-events:none;
    user-select:none
  }

  .chart-label{
    position:absolute;
    z-index:2;

    transform:
      translate(-50%,-50%);

    width:23%;

    text-align:center;

    font-family:
      Arial,
      Helvetica,
      sans-serif;

    font-size:
      clamp(10px,1.5vw,18px);

    font-weight:900;
    line-height:1;
    letter-spacing:.01em;

    color:
      rgba(255,255,255,.96);

    -webkit-text-fill-color:
      rgba(255,255,255,.96);

    text-shadow:
      0 1px 2px rgba(255,255,255,.26),
      0 0 7px rgba(0,0,0,.25);

    pointer-events:none;
    user-select:none
  }

  .marker{
    position:absolute;
    z-index:4;

    width:30px;
    height:30px;

    border-radius:50%;

    transform:
      translate(-50%,-50%);

    background:#071014;

    border:3px solid;

    box-shadow:
      0 0 0 4px rgba(7,16,20,.30),
      0 0 28px rgba(255,255,255,.55),
      0 0 52px rgba(255,255,255,.22)
  }

  .marker-label{
    position:absolute;
    z-index:5;

    transform:
      translate(14px,-48px);

    padding:7px 10px;

    border-radius:999px;

    background:
      rgba(4,11,14,.88);

    border:
      1px solid rgba(255,255,255,.24);

    font-size:11px;
    font-weight:900;

    white-space:nowrap;

    box-shadow:
      0 8px 20px rgba(0,0,0,.24)
  }

  .score-grid{
    display:grid;

    grid-template-columns:
      repeat(2,1fr);

    gap:12px;

    margin-top:26px
  }

  .score-card{
    padding:16px 18px;

    border-radius:20px;

    border:
      1px solid rgba(255,255,255,.12);

    background:
      rgba(255,255,255,.04);

    box-shadow:
      0 12px 28px rgba(0,0,0,.18)
  }

  .score-card h3{
    margin:0 0 10px;

    font:
      900 14px/1
      Arial,
      sans-serif;

    letter-spacing:.12em;
    text-transform:uppercase
  }

  .score-card p{
    margin:4px 0;

    font-size:13px;
    font-weight:800
  }

  .definitions-grid{
    display:grid;

    grid-template-columns:
      repeat(2,1fr);

    gap:12px;

    margin-top:22px
  }

  .definition-card{
    padding:18px;

    border-radius:20px;

    border:
      1px solid rgba(255,255,255,.12);

    background:
      linear-gradient(
        135deg,
        rgba(38,69,151,.18),
        rgba(255,255,255,.03)
      );

    box-shadow:
      0 12px 28px rgba(0,0,0,.16)
  }

  .definition-card h3{
    margin:0 0 8px;

    font:
      900 14px/1
      Arial,
      sans-serif;

    letter-spacing:.14em;
    text-transform:uppercase
  }

  .definition-card p{
    margin:0;

    font:
      600 14px/1.5
      Georgia,
      serif
  }

  .actions{
    display:flex;

    justify-content:center;

    gap:10px;

    flex-wrap:wrap;

    margin-top:26px
  }

  .action-button.primary{
    background:
      linear-gradient(
        90deg,
        #4c80ff,
        #32d4c0,
        #88e752
      )
  }

  .action-button.secondary{
    background:
      rgba(255,255,255,.06)
  }

  .music-toggle{
    position:fixed;

    right:16px;
    bottom:16px;

    z-index:20;

    border:
      1px solid rgba(255,255,255,.18);

    border-radius:999px;

    padding:10px 14px;

    background:
      rgba(4,11,14,.86);

    color:#dff7f2;

    font-family:
      Arial,
      Helvetica,
      sans-serif;

    font-size:11px;
    font-weight:900;
    letter-spacing:.08em;

    cursor:pointer;

    box-shadow:
      0 10px 26px rgba(0,0,0,.28);

    backdrop-filter:blur(8px);

    touch-action:manipulation
  }

  .results-screen .result-title{
    background:
      linear-gradient(
        90deg,
        #72d8ff,
        #9e82ff,
        #ff73c8,
        #ffe36a,
        #70ed82
      );

    -webkit-background-clip:text;
    background-clip:text;

    color:transparent;
    -webkit-text-fill-color:transparent
  }

  .results-screen .result-name{
    background:
      linear-gradient(
        90deg,
        #6ddcff,
        #a678ff,
        #ff70c7,
        #ffd85c,
        #74e67c
      );

    -webkit-background-clip:text;
    background-clip:text;

    color:transparent;
    -webkit-text-fill-color:transparent
  }

  .results-screen .result-sub{
    color:#75e7ff;
    -webkit-text-fill-color:#75e7ff
  }

  .results-screen .chart-heading{
    background:
      linear-gradient(
        90deg,
        #74dcff,
        #9f7cff,
        #ff76cb,
        #ffe36a,
        #70e782
      );

    -webkit-background-clip:text;
    background-clip:text;

    color:transparent;
    -webkit-text-fill-color:transparent
  }

  .results-screen .axis-title{
    background:
      linear-gradient(
        90deg,
        #6adfff,
        #91a2ff,
        #ff71c9,
        #ffe168,
        #78ea7c
      );

    -webkit-background-clip:text;
    background-clip:text;

    color:transparent;
    -webkit-text-fill-color:transparent
  }

  .results-screen .score-card h3{
    background:
      linear-gradient(
        90deg,
        #6edcff,
        #ff7bc9,
        #ffe16b
      );

    -webkit-background-clip:text;
    background-clip:text;

    color:transparent;
    -webkit-text-fill-color:transparent
  }

  .results-screen .score-card p{
    background:
      linear-gradient(
        90deg,
        #82e8ff,
        #c18bff,
        #ff9bd4,
        #ffe888,
        #8ef08f
      );

    -webkit-background-clip:text;
    background-clip:text;

    color:transparent;
    -webkit-text-fill-color:transparent
  }

  .results-screen .definition-card h3{
    filter:
      brightness(1.18)
      saturate(1.25)
  }

  .results-screen .definition-card p{
    background:
      linear-gradient(
        90deg,
        #79ddff,
        #ad93ff,
        #ff8bcb,
        #ffe58a,
        #83e990
      );

    -webkit-background-clip:text;
    background-clip:text;

    color:transparent;
    -webkit-text-fill-color:transparent
  }

  .results-screen .action-button{
    color:#7de5ff;
    -webkit-text-fill-color:#7de5ff
  }

  .results-screen .action-button.primary{
    color:#79ff9d;
    -webkit-text-fill-color:#79ff9d
  }

  .results-screen .chart-label{
    color:
      rgba(255,255,255,.96);

    -webkit-text-fill-color:
      rgba(255,255,255,.96);

    background:none
  }

  @media(max-width:780px){

    .screen{
      padding:14px 7px
    }

    .card{
      width:100%;

      padding:
        18px 10px 22px;

      border-radius:24px
    }

    .top-line{
      align-items:flex-start
    }

    .choices{
      grid-template-columns:
        1fr 1fr
    }

    .choice:last-child{
      grid-column:1/-1
    }

    .chart-frame{
      grid-template-columns:
        24px minmax(0,1fr) 24px;

      grid-template-rows:
        34px auto 34px
    }

    .chart-wrap{
      width:
        min(
          100%,
          calc(100vw - 64px)
        )
    }

    .axis-title{
      font-size:10px;
      letter-spacing:.08em
    }

    .marker{
      width:24px;
      height:24px
    }

    .marker-label{
      font-size:9px;

      transform:
        translate(10px,-39px)
    }

    .chart-label{
      font-size:
        clamp(7px,2.4vw,13px);

      width:23%
    }

    .score-grid,
    .definitions-grid{
      grid-template-columns:1fr
    }

    .music-toggle{
      right:10px;
      bottom:10px;

      padding:9px 12px;
      font-size:10px
    }
  }

  @media(max-width:380px){

    .chart-wrap{
      width:
        min(
          100%,
          calc(100vw - 54px)
        )
    }

    .q-text{
      font-size:21px
    }

    .choice{
      min-height:100px
    }

    .chart-label{
      font-size:7px
    }
  }

  `;

  document.head.appendChild(style);
}

function createInterface(){

  screens.intro=
    createDiv("");

  screens.question=
    createDiv("");

  screens.results=
    createDiv("");

  screens.intro.addClass(
    "screen"
  );

  screens.question.addClass(
    "screen"
  );

  screens.results.addClass(
    "screen"
  );

  screens.results.addClass(
    "results-screen"
  );

  createIntro();
  createQuestion();
  createResults();
  createMusicToggle();

  positionElements();
}

function makeCard(
  parent,
  extraClass=""
){

  const card=
    createDiv("");

  card.parent(parent);

  card.addClass(
    "card"
  );

  if(extraClass){
    card.addClass(
      extraClass
    );
  }

  return card;
}

function createIntro(){

  const card=
    makeCard(
      screens.intro,
      "intro-card"
    );

  const eyebrow=
    createDiv(
      "THREE-CHART COORDINATE TEST"
    );

  eyebrow.parent(card);
  eyebrow.addClass(
    "eyebrow"
  );

  const brand=
    createDiv(
      "NOETIC PHENOTYPES"
    );

  brand.parent(card);
  brand.addClass(
    "intro-brand"
  );

  const pills=
    createDiv("");

  pills.parent(card);
  pills.addClass(
    "axis-pills"
  );

  const group1=
    createDiv(
      "CHART I<br><small>Asemcel ↔ Lexicel · Amodalcel ↕ Aisthecel</small>"
    );

  group1.parent(pills);
  group1.addClass(
    "axis-group"
  );

  group1.addClass(
    "group-1"
  );

  const group2=
    createDiv(
      "CHART II<br><small>Enargcel ↔ Evanescel · Atopocel ↕ Situcel</small>"
    );

  group2.parent(pills);
  group2.addClass(
    "axis-group"
  );

  group2.addClass(
    "group-2"
  );

  const group3=
    createDiv(
      "CHART III<br><small>Abstract ↔ Ephemeral · Obscure ↕ Salient</small>"
    );

  group3.parent(pills);
  group3.addClass(
    "axis-group"
  );

  group3.addClass(
    "group-3"
  );

  const start=
    createButton(
      "ENTER THE MATRIX"
    );

  start.parent(card);

  start.addClass(
    "start-button"
  );

  start.mousePressed(
    ()=>{
      startMusic();
      startTest();
    }
  );

  const tiny=
    createDiv(
      "48 statements · 1–5 to answer · ← to go back · Enter to start"
    );

  tiny.parent(card);

  tiny.addClass(
    "keyboard-help"
  );
}

function createQuestion(){

  const card=
    makeCard(
      screens.question
    );

  const topLine=
    createDiv("");

  topLine.parent(card);

  topLine.addClass(
    "top-line"
  );

  qNum=
    createDiv(
      "QUESTION 01 / 48"
    );

  qNum.parent(
    topLine
  );

  qNum.addClass(
    "badge"
  );

  qAxis=
    createDiv("");

  qAxis.parent(
    topLine
  );

  qAxis.addClass(
    "badge"
  );

  const progress=
    createDiv("");

  progress.parent(card);

  progress.addClass(
    "progress"
  );

  progressBar=
    createDiv("");

  progressBar.parent(
    progress
  );

  progressBar.addClass(
    "progress-bar"
  );

  qText=
    createDiv("");

  qText.parent(card);

  qText.addClass(
    "q-text"
  );

  choices=
    createDiv("");

  choices.parent(card);

  choices.addClass(
    "choices"
  );

  const nav=
    createDiv("");

  nav.parent(card);

  nav.addClass(
    "question-nav"
  );

  backButton=
    createButton(
      "← BACK"
    );

  backButton.parent(nav);

  backButton.addClass(
    "back-button"
  );

  backButton.mousePressed(
    ()=>{
      if(current>0){
        current--;
        renderQuestion();
      }
    }
  );

  const help=
    createDiv(
      "Pick one · the test advances automatically"
    );

  help.parent(nav);

  help.addClass(
    "question-help"
  );
}

function createResults(){

  const card=
    makeCard(
      screens.results
    );

  const title=
    createDiv(
      "YOUR NOETIC PHENOTYPES"
    );

  title.parent(card);

  title.addClass(
    "result-title"
  );

  resultName=
    createDiv("");

  resultName.parent(card);

  resultName.addClass(
    "result-name"
  );

  resultSub=
    createDiv("");

  resultSub.parent(card);

  resultSub.addClass(
    "result-sub"
  );


  const section1=
    createDiv("");

  section1.parent(card);

  section1.addClass(
    "chart-section"
  );

  const heading1=
    createDiv(
      "CHART I"
    );

  heading1.parent(
    section1
  );

  heading1.addClass(
    "chart-heading"
  );

  const frame1=
    createChartFrame(
      section1,
      1
    );

  chartWrap1=
    frame1.wrap;

  chart1=
    frame1.chart;

  createChartImage(
    chart1
  );

  createZoneLabels(
    chart1,
    CHART1_ZONES
  );

  const marker1=
    createMarker(
      chart1
    );

  frame1.marker=
    marker1;

  frame1.markerLabel=
    createMarkerLabel(
      chart1
    );


  const section2=
    createDiv("");

  section2.parent(card);

  section2.addClass(
    "chart-section"
  );

  const heading2=
    createDiv(
      "CHART II"
    );

  heading2.parent(
    section2
  );

  heading2.addClass(
    "chart-heading"
  );

  const frame2=
    createChartFrame(
      section2,
      2
    );

  chartWrap2=
    frame2.wrap;

  chart2=
    frame2.chart;

  createChartImage(
    chart2
  );

  createZoneLabels(
    chart2,
    CHART2_ZONES
  );

  const marker2=
    createMarker(
      chart2
    );

  frame2.marker=
    marker2;

  frame2.markerLabel=
    createMarkerLabel(
      chart2
    );


  const section3=
    createDiv("");

  section3.parent(card);

  section3.addClass(
    "chart-section"
  );

  const heading3=
    createDiv(
      "CHART III"
    );

  heading3.parent(
    section3
  );

  heading3.addClass(
    "chart-heading"
  );

  const frame3=
    createChartFrame(
      section3,
      3
    );

  chartWrap3=
    frame3.wrap;

  chart3=
    frame3.chart;

  createChartImage(
    chart3
  );

  createZoneLabels(
    chart3,
    CHART3_ZONES
  );

  const marker3=
    createMarker(
      chart3
    );

  frame3.marker=
    marker3;

  frame3.markerLabel=
    createMarkerLabel(
      chart3
    );


  chart1.elt.__marker=
    marker1;

  chart1.elt.__markerLabel=
    frame1.markerLabel;

  chart2.elt.__marker=
    marker2;

  chart2.elt.__markerLabel=
    frame2.markerLabel;

  chart3.elt.__marker=
    marker3;

  chart3.elt.__markerLabel=
    frame3.markerLabel;


  scores=
    createDiv("");

  scores.parent(card);

  scores.addClass(
    "score-grid"
  );


  definitionsGrid=
    createDiv("");

  definitionsGrid.parent(
    card
  );

  definitionsGrid.addClass(
    "definitions-grid"
  );


  const actions=
    createDiv("");

  actions.parent(card);

  actions.addClass(
    "actions"
  );


  const retake=
    createButton(
      "↻ RETAKE"
    );

  retake.parent(actions);

  retake.addClass(
    "action-button"
  );

  retake.addClass(
    "primary"
  );

  retake.mousePressed(
    ()=>{
      startMusic();
      startTest();
    }
  );


  const reroll=
    createButton(
      "✦ RE-RANDOMIZE QUESTIONS"
    );

  reroll.parent(actions);

  reroll.addClass(
    "action-button"
  );

  reroll.addClass(
    "secondary"
  );

  reroll.mousePressed(
    ()=>{
      startMusic();
      startTest();
    }
  );


  const menu=
    createButton(
      "⌂ RETURN TO MENU"
    );

  menu.parent(actions);

  menu.addClass(
    "action-button"
  );

  menu.addClass(
    "secondary"
  );

  menu.mousePressed(
    ()=>{
      showScreen(
        "intro"
      );
    }
  );
}

function createChartFrame(
  parent,
  chartNumber
){

  const frame=
    createDiv("");

  frame.parent(parent);

  frame.addClass(
    "chart-frame"
  );

  let topText;
  let leftText;
  let rightText;
  let bottomText;


  if(chartNumber===1){

    topText=
      "AISTHECEL";

    leftText=
      "ASEMCEL";

    rightText=
      "LEXICEL";

    bottomText=
      "AMODALCEL";

  }else if(
    chartNumber===2
  ){

    topText=
      "SITUCEL";

    leftText=
      "ENARGCEL";

    rightText=
      "EVANESCEL";

    bottomText=
      "ATOPOCEL";

  }else{

    topText=
      "OBSCURE";

    leftText=
      "ABSTRACT";

    rightText=
      "EPHEMERAL";

    bottomText=
      "SALIENT";
  }


  const top=
    createDiv(
      topText
    );

  top.parent(frame);

  top.style(
    "grid-column",
    "2"
  );

  top.style(
    "grid-row",
    "1"
  );

  top.addClass(
    "axis-title"
  );


  const left=
    createDiv(
      leftText
    );

  left.parent(frame);

  left.style(
    "grid-column",
    "1"
  );

  left.style(
    "grid-row",
    "2"
  );

  left.addClass(
    "axis-title"
  );

  left.addClass(
    "vertical-axis"
  );

  left.addClass(
    "reverse"
  );


  const wrap=
    createDiv("");

  wrap.parent(frame);

  wrap.addClass(
    "chart-wrap"
  );


  const chart=
    createDiv("");

  chart.parent(wrap);

  chart.addClass(
    "chart"
  );


  const right=
    createDiv(
      rightText
    );

  right.parent(frame);

  right.style(
    "grid-column",
    "3"
  );

  right.style(
    "grid-row",
    "2"
  );

  right.addClass(
    "axis-title"
  );

  right.addClass(
    "vertical-axis"
  );


  const bottom=
    createDiv(
      bottomText
    );

  bottom.parent(frame);

  bottom.style(
    "grid-column",
    "2"
  );

  bottom.style(
    "grid-row",
    "3"
  );

  bottom.addClass(
    "axis-title"
  );


  return{
    frame,
    wrap,
    chart
  };
}

function createChartImage(
  parent
){

  const image=
    createImg(
      CHART_IMAGE,
      "Noetic phenotype chart"
    );

  image.parent(parent);

  image.addClass(
    "chart-image"
  );

  image.elt.draggable=false;
}

function createZoneLabels(
  parent,
  zones
){

  const rows=
    zones.length;

  const cols=
    zones[0].length;


  zones.forEach(
    (row,rowIndex)=>{

      row.forEach(
        (label,colIndex)=>{

          const cell=
            createDiv(
              label
            );

          cell.parent(parent);

          cell.addClass(
            "chart-label"
          );

          cell.style(
            "left",
            `${((colIndex+.5)/cols)*100}%`
          );

          cell.style(
            "top",
            `${((rowIndex+.5)/rows)*100}%`
          );
        }
      );
    }
  );
}

function createMarker(
  parent
){

  const marker=
    createDiv("");

  marker.parent(parent);

  marker.addClass(
    "marker"
  );

  return marker;
}

function createMarkerLabel(
  parent
){

  const label=
    createDiv("");

  label.parent(parent);

  label.addClass(
    "marker-label"
  );

  return label;
}

function createMusicAudio(){

  if(musicAudio){
    return;
  }

  musicAudio=
    document.createElement(
      "audio"
    );

  musicAudio.preload=
    "auto";

  musicAudio.loop=true;

  musicAudio.controls=false;

  musicAudio.volume=
    musicMuted?0:1;

  musicAudio.setAttribute(
    "playsinline",
    ""
  );

  musicAudio.style.position=
    "fixed";

  musicAudio.style.width=
    "1px";

  musicAudio.style.height=
    "1px";

  musicAudio.style.left=
    "-10px";

  musicAudio.style.top=
    "-10px";

  musicAudio.style.opacity=
    "0";

  musicAudio.style.pointerEvents=
    "none";

  musicAudio.src=
    MUSIC_URL;


  musicAudio.addEventListener(
    "error",
    ()=>{
      if(
        musicAudio.src!==
        RAW_MUSIC_URL
      ){

        console.warn(
          "jsDelivr audio failed. Trying GitHub raw audio."
        );

        musicAudio.src=
          RAW_MUSIC_URL;

        musicAudio.load();

        if(musicStarted){
          playNativeMusic();
        }
      }
    }
  );


  document.body.appendChild(
    musicAudio
  );

  musicAudio.load();
}

function playNativeMusic(){

  if(!musicAudio){
    return;
  }

  musicAudio.loop=true;

  musicAudio.volume=
    musicMuted?0:1;

  const promise=
    musicAudio.play();

  if(
    promise!==undefined
  ){

    promise.catch(
      error=>{

        console.error(
          "Native audio playback failed:",
          error
        );

        setTimeout(
          ()=>{
            if(
              musicStarted&&
              musicAudio
            ){

              musicAudio.play()
                .catch(
                  retryError=>{
                    console.error(
                      "Second native audio playback attempt failed:",
                      retryError
                    );
                  }
                );
            }
          },
          500
        );
      }
    );
  }
}

function stopNativeMusic(){

  if(!musicAudio){
    return;
  }

  musicAudio.pause();

  try{
    musicAudio.currentTime=0;
  }catch(e){}
}

function startMusic(){

  musicStarted=true;

  createMusicAudio();

  try{
    userStartAudio();
  }catch(e){}

  if(
    soundLoaded&&
    mySound
  ){

    try{

      mySound.setVolume(
        musicMuted?0:1
      );

      if(
        !mySound.isPlaying()
      ){

        stopNativeMusic();

        mySound.loop();
      }

      return;

    }catch(e){

      console.warn(
        "p5.sound playback failed. Falling back to native audio.",
        e
      );
    }
  }

  playNativeMusic();
}

function toggleMusic(){

  musicMuted=
    !musicMuted;

  if(
    mySound&&
    soundLoaded
  ){

    mySound.setVolume(
      musicMuted?0:1
    );
  }

  if(musicAudio){

    musicAudio.volume=
      musicMuted?0:1;
  }

  if(musicToggle){

    musicToggle.html(
      musicMuted
        ?"♫ MUSIC OFF"
        :"♫ MUSIC ON"
    );
  }
}

function createMusicToggle(){

  musicToggle=
    createButton(
      "♫ MUSIC ON"
    );

  musicToggle.addClass(
    "music-toggle"
  );

  musicToggle.mousePressed(
    ()=>{
      toggleMusic();
    }
  );
}

function startTest(){

  current=0;

  responses=
    new Array(
      QUESTIONS.length
    ).fill(null);

  questions=
    QUESTIONS.slice();

  renderQuestion();

  showScreen(
    "question"
  );
}

function selectAnswer(
  index
){

  if(
    !screens.question||
    screens.question.elt.style.display==="none"
  ){
    return;
  }

  if(
    index<0||
    index>=ANSWERS.length
  ){
    return;
  }

  responses[current]=
    index;

  renderQuestion();

  setTimeout(
    ()=>{
      if(
        screens.question&&
        screens.question.elt.style.display!=="none"
      ){

        if(
          current<
          questions.length-1
        ){

          current++;

          renderQuestion();

        }else{

          finishTest();
        }
      }
    },
    120
  );
}

function renderQuestion(){

  const item=
    questions[current];

  qNum.html(
    `QUESTION ${
      String(
        current+1
      ).padStart(2,"0")
    } / ${
      questions.length
    }`
  );

  qAxis.html(
    getAxisLabel(
      item[0]
    )
  );

  qText.html(
    item[1]
  );

  progressBar.style(
    "width",
    `${
      (
        current/
        questions.length
      )*100
    }%`
  );

  choices.html("");

  ANSWERS.forEach(
    (answer,index)=>{

      const button=
        createButton(
          `<div class="choice-number">${index+1}</div><div class="choice-label">${answer}</div>`
        );

      button.parent(
        choices
      );

      button.addClass(
        "choice"
      );

      if(
        responses[current]===
        index
      ){

        button.addClass(
          "selected"
        );
      }

      button.mousePressed(
        ()=>{
          selectAnswer(
            index
          );
        }
      );
    }
  );

  backButton.style(
    "visibility",
    current>0
      ?"visible"
      :"hidden"
  );

  positionElements();
}

function getAxisLabel(
  key
){

  if(
    key==="aisthecel"||
    key==="amodalcel"
  ){

    return "CHART I · SENSORY";
  }

  if(
    key==="situcel"||
    key==="atopocel"
  ){

    return "CHART II · SPATIAL";
  }

  if(
    key==="enargcel"||
    key==="evanescel"
  ){

    return "CHART II · OPACITY";
  }

  if(
    key==="lexicel"||
    key==="asemcel"
  ){

    return "CHART I · SEMANTIC";
  }

  if(
    key==="abstractcel"||
    key==="ephemeralcel"
  ){

    return "CHART III · MATERIALITY";
  }

  if(
    key==="obscurecel"||
    key==="salientcel"
  ){

    return "CHART III · FOCALITY";
  }

  return "";
}

function finishTest(){

  calculateScores();

  showScreen(
    "results"
  );

  positionElements();
}

function calculateScores(){

  const totals={};
  const counts={};

  Object.keys(
    DEFINITIONS
  ).forEach(
    key=>{

      totals[key]=0;
      counts[key]=0;
    }
  );


  questions.forEach(
    (item,index)=>{

      const key=
        item[0];

      const answer=
        responses[index];

      if(
        answer===null||
        answer===undefined
      ){

        return;
      }

      // 1=-2, 2=-1, 3=0, 4=+1, 5=+2.

      totals[key]+=
        answer-2;

      counts[key]++;
    }
  );


  const poleAverage=(
    key
  )=>{

    const count=
      counts[key]||0;

    return count>0
      ?totals[key]/count
      :0;
  };


  const axisPercent=(
    firstKey,
    secondKey
  )=>{

    const first=
      poleAverage(
        firstKey
      );

    const second=
      poleAverage(
        secondKey
      );

    const net=
      second-first;

    return netToPercent(
      net,
      -4,
      4
    );
  };


  const chart1X=
    axisPercent(
      "asemcel",
      "lexicel"
    );

  const chart1Y=
    axisToScreenY(
      axisPercent(
        "amodalcel",
        "aisthecel"
      )
    );


  const chart2X=
    axisPercent(
      "enargcel",
      "evanescel"
    );

  const chart2Y=
    axisToScreenY(
      axisPercent(
        "atopocel",
        "situcel"
      )
    );


  const chart3X=
    axisPercent(
      "abstractcel",
      "ephemeralcel"
    );

  /*
    IMPORTANT:
    Chart III vertical direction is:

      OBSCURE = UP
      SALIENT = DOWN

    Therefore the scoring comparison is
    intentionally reversed here.
  */

  const chart3Y=
    axisToScreenY(
      axisPercent(
        "salientcel",
        "obscurecel"
      )
    );


  const chart1Zone=
    getChartZone(
      CHART1_ZONES,
      chart1X,
      chart1Y
    );

  const chart2Zone=
    getChartZone(
      CHART2_ZONES,
      chart2X,
      chart2Y
    );

  const chart3Zone=
    getChartZone(
      CHART3_ZONES,
      chart3X,
      chart3Y
    );


  scores.html(`

    <div class="score-card">

      <h3>CHART I</h3>

      <p>
        SEMANTIC:
        ${formatAxisScore(
          totals.asemcel,
          totals.lexicel,
          "ASEMCEL",
          "LEXICEL",
          counts.asemcel,
          counts.lexicel
        )}
      </p>

      <p>
        SENSORY:
        ${formatAxisScore(
          totals.aisthecel,
          totals.amodalcel,
          "AISTHECEL",
          "AMODALCEL",
          counts.aisthecel,
          counts.amodalcel
        )}
      </p>

    </div>


    <div class="score-card">

      <h3>CHART II</h3>

      <p>
        OPACITY:
        ${formatAxisScore(
          totals.enargcel,
          totals.evanescel,
          "ENARGCEL",
          "EVANESCEL",
          counts.enargcel,
          counts.evanescel
        )}
      </p>

      <p>
        SPATIAL:
        ${formatAxisScore(
          totals.situcel,
          totals.atopocel,
          "SITUCEL",
          "ATOPOCEL",
          counts.situcel,
          counts.atopocel
        )}
      </p>

    </div>


    <div class="score-card">

      <h3>CHART III</h3>

      <p>
        ABSTRACTNESS:
        ${formatAxisScore(
          totals.abstractcel,
          totals.ephemeralcel,
          "ABSTRACT",
          "EPHEMERAL",
          counts.abstractcel,
          counts.ephemeralcel
        )}
      </p>

      <p>
        FOCALITY:
        ${formatAxisScore(
          totals.obscurecel,
          totals.salientcel,
          "OBSCURE",
          "SALIENT",
          counts.obscurecel,
          counts.salientcel
        )}
      </p>

    </div>

  `);


  /*
    Internal keys are used for scoring.
    Display names are explicitly separated so
    Chart III does not gain an unwanted "CEL".
  */

  const defs=[

    ["ASEMCEL","asemcel"],
    ["LEXICEL","lexicel"],
    ["AISTHECEL","aisthecel"],
    ["AMODALCEL","amodalcel"],

    ["ENARGCEL","enargcel"],
    ["EVANESCEL","evanescel"],
    ["ATOPOCEL","atopocel"],
    ["SITUCEL","situcel"],

    ["ABSTRACT","abstractcel"],
    ["EPHEMERAL","ephemeralcel"],
    ["OBSCURE","obscurecel"],
    ["SALIENT","salientcel"]
  ];


  definitionsGrid.html("");


  defs.forEach(
    ([displayName,key])=>{

      const card=
        createDiv(
          `<h3>${displayName}</h3><p>${DEFINITIONS[key]}</p>`
        );

      card.parent(
        definitionsGrid
      );

      card.addClass(
        "definition-card"
      );
    }
  );


  positionMarker(
    chart1,
    chart1X,
    chart1Y,
    chart1Zone
  );

  positionMarker(
    chart2,
    chart2X,
    chart2Y,
    chart2Zone
  );

  positionMarker(
    chart3,
    chart3X,
    chart3Y,
    chart3Zone
  );


  const dominantNames=[

    getDominantName(
      poleAverage("asemcel"),
      "ASEMCEL",
      poleAverage("lexicel"),
      "LEXICEL"
    ),

    getDominantName(
      poleAverage("aisthecel"),
      "AISTHECEL",
      poleAverage("amodalcel"),
      "AMODALCEL"
    ),

    getDominantName(
      poleAverage("enargcel"),
      "ENARGCEL",
      poleAverage("evanescel"),
      "EVANESCEL"
    ),

    getDominantName(
      poleAverage("situcel"),
      "SITUCEL",
      poleAverage("atopocel"),
      "ATOPOCEL"
    ),

    getDominantName(
      poleAverage("abstractcel"),
      "ABSTRACT",
      poleAverage("ephemeralcel"),
      "EPHEMERAL"
    ),

    getDominantName(
      poleAverage("obscurecel"),
      "OBSCURE",
      poleAverage("salientcel"),
      "SALIENT"
    )
  ];


  resultName.html(
    dominantNames.join(
      " · "
    )
  );


  resultSub.html(
    `CHART I · ${chart1Zone}<br>
     CHART II · ${chart2Zone}<br>
     CHART III · ${chart3Zone}`
  );
}

function axisToScreenY(
  percent
){

  return 100-percent;
}

function netToPercent(
  net,
  minNet=-32,
  maxNet=32
){

  return (
    (net-minNet)/
    (maxNet-minNet)
  )*100;
}

function getChartZone(
  zones,
  xPercent,
  yPercent
){

  const rows=
    zones.length;

  const cols=
    zones[0].length;


  const x=Math.max(
    0,
    Math.min(
      99.999999,
      xPercent
    )
  );

  const y=Math.max(
    0,
    Math.min(
      99.999999,
      yPercent
    )
  );


  const col=
    Math.floor(
      (x/100)*cols
    );

  const row=
    Math.floor(
      (y/100)*rows
    );


  return zones[row][col];
}

function formatAxisScore(
  first,
  second,
  firstName,
  secondName,
  firstCount=0,
  secondCount=0
){

  const firstTotal=
    first||0;

  const secondTotal=
    second||0;


  const firstAvg=
    firstCount>0
      ?firstTotal/firstCount
      :0;

  const secondAvg=
    secondCount>0
      ?secondTotal/secondCount
      :0;


  const percent=
    Math.round(
      netToPercent(
        secondAvg-firstAvg,
        -4,
        4
      )
    );


  return `${
    firstName
  } ${
    firstAvg.toFixed(2)
  } ↔ ${
    secondName
  } ${
    secondAvg.toFixed(2)
  } · ${
    percent
  }%`;
}

function mapValue(
  left,
  right
){

  const total=
    left+right;

  if(total<=0){
    return 50;
  }

  return (
    right/total
  )*100;
}

function getDominant(
  left,
  right
){

  if(left>right){
    return "LEFT";
  }

  if(right>left){
    return "RIGHT";
  }

  return "CENTER";
}

function getDominantName(
  first,
  firstName,
  second,
  secondName
){

  if(first>second){
    return firstName;
  }

  if(second>first){
    return secondName;
  }

  return "BALANCED";
}

function positionMarker(
  chart,
  xPercent,
  yPercent,
  label
){

  const marker=
    chart.elt.__marker;

  const markerLabel=
    chart.elt.__markerLabel;


  marker.style(
    "left",
    `${xPercent}%`
  );

  marker.style(
    "top",
    `${yPercent}%`
  );


  markerLabel.html(
    label
  );


  markerLabel.style(
    "left",
    `${xPercent}%`
  );

  markerLabel.style(
    "top",
    `${yPercent}%`
  );
}

function showScreen(
  name
){

  Object.keys(
    screens
  ).forEach(
    key=>{

      screens[key].style(
        "display",
        key===name
          ?"block"
          :"none"
      );
    }
  );

  positionElements();
}

function positionElements(){

  if(
    !screens.results||
    screens.results.elt.style.display==="none"
  ){

    return;
  }


  if(
    chart1&&
    chart1.elt.__marker
  ){

    const marker1=
      chart1.elt.__marker;

    const markerLabel1=
      chart1.elt.__markerLabel;


    marker1.style(
      "position",
      "absolute"
    );

    markerLabel1.style(
      "position",
      "absolute"
    );
  }


  if(
    chart2&&
    chart2.elt.__marker
  ){

    const marker2=
      chart2.elt.__marker;

    const markerLabel2=
      chart2.elt.__markerLabel;


    marker2.style(
      "position",
      "absolute"
    );

    markerLabel2.style(
      "position",
      "absolute"
    );
  }


  if(
    chart3&&
    chart3.elt.__marker
  ){

    const marker3=
      chart3.elt.__marker;

    const markerLabel3=
      chart3.elt.__markerLabel;


    marker3.style(
      "position",
      "absolute"
    );

    markerLabel3.style(
      "position",
      "absolute"
    );
  }
}