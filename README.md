# IKS_MCQ
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>IKS Mock Test</title>

<style>
*{
margin:0;
padding:0;
box-sizing:border-box;
font-family:'Segoe UI',sans-serif;
}

body{
background:linear-gradient(135deg,#6a11cb,#2575fc);
min-height:100vh;
padding:20px;
}

.container{
max-width:1000px;
margin:auto;
}

.header{
text-align:center;
color:white;
margin-bottom:20px;
}

.card{
background:white;
border-radius:20px;
padding:25px;
box-shadow:0 10px 25px rgba(0,0,0,.2);
}

.progress-container{
height:12px;
background:#ddd;
border-radius:20px;
overflow:hidden;
margin-bottom:20px;
}

.progress{
height:100%;
width:0%;
background:linear-gradient(90deg,#00c853,#64dd17);
transition:.3s;
}

.question-number{
font-size:18px;
font-weight:bold;
color:#2575fc;
margin-bottom:10px;
}

.question{
font-size:22px;
font-weight:600;
margin-bottom:20px;
}

.option{
padding:15px;
margin:10px 0;
border:2px solid #ddd;
border-radius:12px;
cursor:pointer;
transition:.3s;
}

.option:hover{
background:#eef5ff;
border-color:#2575fc;
}

.selected{
background:#2575fc;
color:white;
border-color:#2575fc;
}

.buttons{
display:flex;
justify-content:space-between;
margin-top:20px;
}

button{
padding:12px 25px;
border:none;
border-radius:10px;
font-size:16px;
font-weight:bold;
cursor:pointer;
}

.prev{
background:#ff9800;
color:white;
}

.next{
background:#2196f3;
color:white;
}

.submit{
background:#4caf50;
color:white;
}

.palette{
display:grid;
grid-template-columns:repeat(auto-fill,minmax(40px,1fr));
gap:8px;
margin-top:25px;
}

.palette button{
padding:10px;
background:#ddd;
color:black;
}

.palette button.answered{
background:#4caf50;
color:white;
}

.result{
display:none;
text-align:center;
}

.score{
font-size:40px;
font-weight:bold;
color:#2575fc;
margin:20px 0;
}

.review{
margin-top:20px;
text-align:left;
}

.correct{
color:green;
font-weight:bold;
}

.wrong{
color:red;
font-weight:bold;
}

@media(max-width:600px){
.question{
font-size:18px;
}
}
</style>
</head>

<body>

<div class="container">

<div class="header">
<h1>🎓 IKS Mock Test</h1>
<p>30 Random Questions</p>
</div>

<div class="card" id="quizCard">

<div class="progress-container">
<div class="progress" id="progress"></div>
</div>

<div class="question-number" id="qNo"></div>
<div class="question" id="question"></div>

<div id="options"></div>

<div class="buttons">
<button class="prev" onclick="prevQuestion()">Previous</button>
<button class="next" onclick="nextQuestion()">Next</button>
<button class="submit" onclick="submitTest()">Submit</button>
</div>

<div class="palette" id="palette"></div>

</div>

<div class="card result" id="resultCard">
<h2>Test Completed</h2>

<div class="score" id="score"></div>

<h3 id="stats"></h3>

<div class="review" id="review"></div>
</div>

</div>

<script>

const questions = [

{
question:"Indian Traditional Knowledge is also referred to as:",
options:[
"Indian Knowledge System",
"Indigenous Knowledge System",
"Independent Knowledge System",
"Intellectual Knowledge System"
],
answer:0
},

{
question:"The term Indian in IKS refers to:",
options:[
"The broader cultural-geographical region of the Indian subcontinent",
"Only post-1947 India",
"A political party",
"Only North India"
],
answer:0
},

{
question:"Knowledge in the Indian tradition emerges through:",
options:[
"Observation and experimentation",
"Blind faith",
"Guesswork",
"Superstition"
],
answer:0
},

{
question:"Ayurveda and Yoga belong to:",
options:[
"Health and Medicine",
"Governance",
"Trade",
"Defense"
],
answer:0
},

{
question:"The Yoga system was systematized by:",
options:[
"Patanjali",
"Kapila",
"Kanada",
"Jaimini"
],
answer:0
},

{
question:"The Iron Pillar of Delhi is an example of:",
options:[
"Metallurgy",
"Temple Art",
"Literature",
"Agriculture"
],
answer:0
},

{
question:"Takshashila was located in present-day:",
options:[
"Pakistan",
"Nepal",
"India",
"Sri Lanka"
],
answer:0
},

{
question:"Nalanda's famous library was called:",
options:[
"Dharmaganja",
"Ratnavali",
"Arthashala",
"Granthagriha"
],
answer:0
},

{
question:"The Gurukula system emphasized:",
options:[
"Moral, spiritual and intellectual development",
"Only vocational skills",
"Only academics",
"Military training alone"
],
answer:0
},

{
question:"The concept of unity in diversity reflects:",
options:[
"Pluralism",
"Isolation",
"Uniformity",
"Rigidity"
],
answer:0
}

];

// Duplicate until enough questions
while(questions.length < 30){
questions.push(...questions.slice(0,10));
}

let quiz = questions
.sort(()=>Math.random()-0.5)
.slice(0,30);

let current=0;
let answers={};

const qNo=document.getElementById("qNo");
const question=document.getElementById("question");
const options=document.getElementById("options");
const palette=document.getElementById("palette");
const progress=document.getElementById("progress");

function createPalette(){
palette.innerHTML="";
quiz.forEach((_,i)=>{
let btn=document.createElement("button");
btn.innerText=i+1;

if(answers[i]!=undefined)
btn.classList.add("answered");

btn.onclick=()=>{
current=i;
loadQuestion();
};

palette.appendChild(btn);
});
}

function loadQuestion(){

let q=quiz[current];

qNo.innerText=`Question ${current+1} of 30`;
question.innerText=q.question;

options.innerHTML="";

q.options.forEach((opt,index)=>{
let div=document.createElement("div");
div.className="option";
div.innerText=opt;

if(answers[current]===index)
div.classList.add("selected");

div.onclick=()=>{
answers[current]=index;
loadQuestion();
createPalette();
};

options.appendChild(div);
});

progress.style.width=
((current+1)/30)*100+"%";
}

function nextQuestion(){
if(current<29){
current++;
loadQuestion();
}
}

function prevQuestion(){
if(current>0){
current--;
loadQuestion();
}
}

function submitTest(){

let correct=0;

quiz.forEach((q,i)=>{
if(answers[i]===q.answer)
correct++;
});

let incorrect=
Object.keys(answers).length-correct;

let unanswered=
30-Object.keys(answers).length;

let percent=
((correct/30)*100).toFixed(2);

let grade="F";

if(percent>=90) grade="A+";
else if(percent>=80) grade="A";
else if(percent>=70) grade="B";
else if(percent>=60) grade="C";
else if(percent>=50) grade="D";

document.getElementById("quizCard").style.display="none";
document.getElementById("resultCard").style.display="block";

document.getElementById("score").innerHTML=
percent+"%";

document.getElementById("stats").innerHTML=
`
Correct: ${correct}<br>
Incorrect: ${incorrect}<br>
Unanswered: ${unanswered}<br>
Grade: ${grade}
`;

let reviewHTML="<h3>Answer Review</h3>";

quiz.forEach((q,i)=>{

reviewHTML+=`
<p>
<b>Q${i+1}:</b> ${q.question}<br>
Your Answer:
${answers[i]!=undefined ?
q.options[answers[i]] :
"Not Answered"}
<br>
Correct Answer:
<span class="correct">
${q.options[q.answer]}
</span>
</p><hr>
`;
});

document.getElementById("review").innerHTML=
reviewHTML;
}

createPalette();
loadQuestion();

</script>

</body>
</html>
