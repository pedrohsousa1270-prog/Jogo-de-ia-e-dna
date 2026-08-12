<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>DNA & IA - Laboratório Genético</title>

<style>
*{box-sizing:border-box}

body{
    margin:0;
    font-family:Arial,sans-serif;
    color:white;
    background:linear-gradient(135deg,#06101d,#102d46,#073d32);
    min-height:100vh;
}

.container{
    max-width:950px;
    margin:auto;
    padding:20px;
}

header{
    text-align:center;
    padding:15px;
}

header h1{
    font-size:38px;
    margin:5px;
}

header p{
    color:#b9d9e8;
}

.card{
    background:rgba(255,255,255,.09);
    border:1px solid rgba(255,255,255,.15);
    border-radius:20px;
    padding:25px;
    margin-bottom:20px;
    box-shadow:0 10px 30px rgba(0,0,0,.3);
}

.hidden{display:none}

button{
    width:100%;
    padding:14px;
    margin:7px 0;
    border:0;
    border-radius:12px;
    background:#19c37d;
    color:white;
    font-size:16px;
    font-weight:bold;
    cursor:pointer;
}

button:hover{
    filter:brightness(1.1);
    transform:scale(1.01);
}

.blue{
    background:#2677ff;
}

.purple{
    background:#7c4dff;
}

.dna{
    text-align:center;
    font-size:42px;
    margin:15px;
}

.info{
    display:flex;
    justify-content:space-between;
    flex-wrap:wrap;
    gap:8px;
}

.info div{
    background:rgba(0,0,0,.25);
    padding:10px 15px;
    border-radius:10px;
}

.question{
    font-size:21px;
    line-height:1.5;
    margin:20px 0;
}

.answer{
    background:rgba(255,255,255,.1);
}

.correct{
    background:#159957!important;
}

.wrong{
    background:#d9363e!important;
}

.feedback{
    background:rgba(0,0,0,.3);
    border-radius:12px;
    padding:15px;
    margin-top:15px;
    line-height:1.5;
}

.progress{
    height:10px;
    background:rgba(255,255,255,.15);
    border-radius:10px;
    overflow:hidden;
    margin:20px 0;
}

.progress-bar{
    height:100%;
    width:0%;
    background:#19c37d;
}

textarea{
    width:100%;
    min-height:90px;
    resize:none;
    border:0;
    border-radius:12px;
    padding:15px;
    font-size:18px;
    background:#071725;
    color:#62ffba;
    font-family:monospace;
    text-transform:uppercase;
}

.resultBox{
    background:rgba(0,0,0,.3);
    padding:18px;
    border-radius:15px;
    margin-top:15px;
}

.sequence{
    font-family:monospace;
    font-size:20px;
    color:#62ffba;
    word-break:break-all;
}

.match{
    color:#19c37d;
    font-weight:bold;
}

.mismatch{
    color:#ff6464;
    font-weight:bold;
}

.percent{
    font-size:42px;
    text-align:center;
    color:#19c37d;
    font-weight:bold;
}

.score{
    text-align:center;
    font-size:48px;
    color:#19c37d;
}

.small{
    color:#b9d9e8;
    font-size:14px;
}
</style>
</head>

<body>

<div class="container">

<header>
    <h1>🧬 DNA & IA</h1>
    <p>Laboratório Genético Virtual</p>
</header>

<!-- MENU -->

<div id="menu" class="card">

    <div class="dna">🧬 🧬 🧬</div>

    <h2>Bem-vindo ao laboratório!</h2>

    <p>
        Aqui você vai aprender sobre <b>DNA, genes, espécies,
        evolução e sequenciamento genético</b>.
    </p>

    <button class="blue" onclick="startGame()">
        🧠 DESAFIO DE GENÉTICA
    </button>

    <button class="purple" onclick="openSequencing()">
        🧬 SEQUENCIAMENTO DE DNA
    </button>

</div>


<!-- JOGO -->

<div id="game" class="card hidden">

    <div class="info">
        <div>❤️ Vidas: <span id="lives">3</span></div>
        <div>⭐ Pontos: <span id="score">0</span></div>
        <div>🧠 Nível: <span id="level">1</span></div>
    </div>

    <div class="progress">
        <div id="progressBar" class="progress-bar"></div>
    </div>

    <div id="question" class="question"></div>

    <div id="answers"></div>

    <div id="feedback" class="feedback hidden"></div>

    <button id="nextButton"
            class="blue hidden"
            onclick="nextQuestion()">
        PRÓXIMA PERGUNTA →
    </button>

    <button onclick="backMenu()">
        VOLTAR AO LABORATÓRIO
    </button>

</div>


<!-- SEQUENCIAMENTO -->

<div id="sequencing" class="card hidden">

    <h2>🧬 Sequenciamento de DNA</h2>

    <p>
        Digite duas sequências de DNA usando apenas as bases
        <b>A, T, C e G</b>.
    </p>

    <p class="small">
        Exemplo: ATGCCATGACTG
    </p>

    <label>🔬 Sequência da espécie A</label>
    <textarea id="dnaA" placeholder="Digite a sequência..."></textarea>

    <br>

    <label>🔬 Sequência da espécie B</label>
    <textarea id="dnaB" placeholder="Digite a sequência..."></textarea>

    <button class="purple" onclick="analyzeDNA()">
        🤖 ANALISAR COM IA
    </button>

    <div id="dnaResult" class="resultBox hidden"></div>

    <button onclick="backMenu()">
        ← VOLTAR
    </button>

</div>


<!-- RESULTADO DO JOGO -->

<div id="result" class="card hidden">

    <div class="dna">🧬</div>

    <h2>Experimento concluído!</h2>

    <p>Sua pontuação:</p>

    <div id="finalScore" class="score">0</div>

    <p id="finalMessage"></p>

    <button onclick="restartGame()">
        🔄 JOGAR NOVAMENTE
    </button>

    <button class="purple" onclick="openSequencing()">
        🧬 TESTAR SEQUENCIAMENTO
    </button>

</div>

</div>


<script>

/* =========================
   PERGUNTAS
========================= */

const questions = [

{
question:"O que é o DNA?",
answers:[
"Uma proteína",
"Uma molécula que armazena informações genéticas",
"Uma célula",
"Um órgão"
],
correct:1,
explanation:"O DNA armazena as informações genéticas dos seres vivos."
},

{
question:"O que é um gene?",
answers:[
"Uma célula",
"Um tipo de proteína",
"Uma região do DNA que possui informação genética",
"Um órgão"
],
correct:2,
explanation:"Genes são regiões do DNA que possuem informações específicas."
},

{
question:"Qual é a principal função do DNA?",
answers:[
"Produzir energia",
"Armazenar e transmitir informações genéticas",
"Transportar oxigênio",
"Digestionar alimentos"
],
correct:1,
explanation:"O DNA armazena e transmite informações genéticas."
},

{
question:"Quais são as quatro bases do DNA?",
answers:[
"A, T, C e G",
"A, B, C e D",
"X, Y, Z e W",
"G, H, I e J"
],
correct:0,
explanation:"As quatro bases são adenina, timina, citosina e guanina."
},

{
question:"Qual base se liga à adenina no DNA?",
answers:[
"Citosina",
"Guanina",
"Timina",
"Uracila"
],
correct:2,
explanation:"No DNA, a adenina se liga à timina."
},

{
question:"Um Labrador e um vira-lata pertencem a qual espécie?",
answers:[
"Espécies completamente diferentes",
"À mesma espécie de cão doméstico",
"À espécie dos gatos",
"Não possuem espécie"
],
correct:1,
explanation:"Labradores e vira-latas são cães domésticos e pertencem à mesma espécie."
},

{
question:"Por que comparar DNA de espécies diferentes?",
answers:[
"Para estudar relações de parentesco e evolução",
"Para mudar o DNA imediatamente",
"Para transformar animais em plantas",
"Para descobrir tudo sobre um organismo"
],
correct:0,
explanation:"Comparações de DNA ajudam a investigar parentesco e evolução."
},

{
question:"O que é uma sequência genética?",
answers:[
"A ordem das bases no DNA",
"O tamanho de um animal",
"A cor de uma célula",
"O formato do núcleo"
],
correct:0,
explanation:"Uma sequência genética representa a ordem das bases A, T, C e G."
},

{
question:"O que significa sequenciar o DNA?",
answers:[
"Descobrir a ordem das bases do DNA",
"Destruir o DNA",
"Transformar DNA em proteína",
"Eliminar os genes"
],
correct:0,
explanation:"Sequenciamento determina a ordem das bases presentes em uma região de DNA."
},

{
question:"Se duas espécies possuem sequências de DNA muito parecidas, o que isso pode indicar?",
answers:[
"Possível relação de parentesco evolutivo",
"Que são exatamente o mesmo indivíduo",
"Que não possuem genes",
"Que uma é necessariamente mais velha"
],
correct:0,
explanation:"Semelhanças genéticas podem indicar uma relação de parentesco evolutivo."
}

];

let currentQuestion=0;
let score=0;
let lives=3;
let level=1;
let answered=false;


/* =========================
   MENU
========================= */

function hideAll(){

document.getElementById("menu").classList.add("hidden");
document.getElementById("game").classList.add("hidden");
document.getElementById("sequencing").classList.add("hidden");
document.getElementById("result").classList.add("hidden");

}


/* =========================
   JOGO
========================= */

function startGame(){

hideAll();

document.getElementById("game").classList.remove("hidden");

currentQuestion=0;
score=0;
lives=3;
level=1;

updateInfo();
showQuestion();

}


function showQuestion(){

answered=false;

let q=questions[currentQuestion];

document.getElementById("question").innerText=
(currentQuestion+1)+". "+q.question;

let answers=document.getElementById("answers");

answers.innerHTML="";

document.getElementById("feedback")
.classList.add("hidden");

document.getElementById("nextButton")
.classList.add("hidden");

q.answers.forEach((answer,index)=>{

let button=document.createElement("button");

button.className="answer";

button.innerText=answer;

button.onclick=()=>checkAnswer(index,button);

answers.appendChild(button);

});

let progress=
(currentQuestion/questions.length)*100;

document.getElementById("progressBar")
.style.width=progress+"%";

}


function checkAnswer(index,button){

if(answered)return;

answered=true;

let q=questions[currentQuestion];

let buttons=document.querySelectorAll(".answer");

buttons.forEach(btn=>{
btn.disabled=true;
});

if(index===q.correct){

button.classList.add("correct");

score+=100*level;

level++;

document.getElementById("feedback").innerHTML=
"✅ <b>Correto!</b><br>"+q.explanation;

}else{

button.classList.add("wrong");

buttons[q.correct].classList.add("correct");

lives--;

document.getElementById("feedback").innerHTML=
"❌ <b>Errado!</b><br>"+q.explanation;

}

updateInfo();

document.getElementById("feedback")
.classList.remove("hidden");

if(lives<=0){

setTimeout(finishGame,1200);

}else{

document.getElementById("nextButton")
.classList.remove("hidden");

}

}


function nextQuestion(){

currentQuestion++;

if(currentQuestion>=questions.length){

finishGame();

return;

}

showQuestion();

}


function updateInfo(){

document.getElementById("lives").innerText=lives;
document.getElementById("score").innerText=score;
document.getElementById("level").innerText=level;

}


function finishGame(){

hideAll();

document.getElementById("result")
.classList.remove("hidden");

document.getElementById("finalScore")
.innerText=score;

let message;

if(score>=900){

message=
"🧬 Excelente! Você é um especialista em genética!";

}else if(score>=600){

message=
"🔬 Muito bom! Você entende bastante de DNA.";

}else if(score>=300){

message=
"🧪 Bom trabalho! Continue estudando.";

}else{

message=
"📚 Continue estudando. Você pode melhorar!";
}

document.getElementById("finalMessage")
.innerText=message;

}


function restartGame(){

startGame();

}


/* =========================
   SEQUENCIAMENTO
========================= */

function openSequencing(){

hideAll();

document.getElementById("sequencing")
.classList.remove("hidden");

document.getElementById("dnaResult")
.classList.add("hidden");

}


function cleanDNA(sequence){

return sequence
.toUpperCase()
.replace(/[^ATCG]/g,"");

}


function analyzeDNA(){

let rawA=document.getElementById("dnaA").value;
let rawB=document.getElementById("dnaB").value;

let A=cleanDNA(rawA);
let B=cleanDNA(rawB);

let result=document.getElementById("dnaResult");

if(A.length===0 || B.length===0){

result.classList.remove("hidden");

result.innerHTML=
"⚠️ Digite as duas sequências de DNA.";

return;

}

let max=Math.max(A.length,B.length);

let matches=0;

let comparison="";

for(let i=0;i<max;i++){

let a=A[i] || "-";
let b=B[i] || "-";

if(a===b && a!=="-"){

matches++;

comparison+=
`<span class="match">${a}</span>`;

}else{

comparison+=
`<span class="mismatch">${a}</span>`;

}

}

let percentage=(matches/max)*100;

let interpretation="";

if(percentage>=90){

interpretation=
"🧬 Sequências extremamente semelhantes.";

}else if(percentage>=70){

interpretation=
"🔬 Sequências bastante semelhantes.";

}else if(percentage>=50){

interpretation=
"🧪 Existe uma semelhança moderada.";

}else{

interpretation=
"🔎 As sequências apresentam muitas diferenças.";

}

result.classList.remove("hidden");

result.innerHTML=`

<h3>🤖 IA analisando...</h3>

<p><b>Sequência A:</b></p>

<p class="sequence">${A}</p>

<p><b>Sequência B:</b></p>

<p class="sequence">${B}</p>

<hr>

<h3>📊 Resultado da comparação</h3>

<div class="percent">
${percentage.toFixed(1)}%
</div>

<p>
<b>${matches}</b> bases iguais de
<b>${max}</b> posições analisadas.
</p>

<p>${interpretation}</p>

<p class="small">
⚠️ Esta é uma comparação didática simplificada.
Na ciência real, análises genéticas usam métodos e
algoritmos muito mais complexos.
</p>

`;

}


/* =========================
   VOLTAR
========================= */

function backMenu(){

hideAll();

document.getElementById("menu")
.classList.remove("hidden");

}

</script>

</body>
</html>
