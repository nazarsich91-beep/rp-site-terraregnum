# rp-site-terraregnum[iddex.htm](https://github.com/user-attachments/files/27521013/iddex.htm)
<!DOCTYPE html>
<html lang="uk">
<head>
<meta charset="UTF-8">
<title>TERRA REGNUM</title>

<style>
body{
margin:0;
font-family:Arial;
background:#0f172a;
color:white;
}

header{
background:linear-gradient(135deg,#1e293b,#334155);
padding:25px;
text-align:center;
font-size:26px;
font-weight:bold;
}

.topbar{
display:flex;
justify-content:space-between;
padding:10px 20px;
}

.langBtn{
width:70px;
font-size:11px;
padding:4px;
border-radius:6px;
border:none;
}

.container{
display:grid;
grid-template-columns:repeat(auto-fill,minmax(320px,1fr));
gap:20px;
padding:20px;
}

.block{
background:#1e293b;
padding:15px;
border-radius:15px;
display:flex;
flex-direction:column;
gap:10px;
position:relative;
}

img{
width:100%;
height:180px;
object-fit:cover;
border-radius:10px;
}

select,input{
width:100%;
padding:8px;
border-radius:8px;
border:none;
box-sizing:border-box;
}

.deleteBtn{
position:absolute;
top:10px;
right:10px;
background:#ef4444;
color:white;
border:none;
border-radius:6px;
padding:5px 8px;
cursor:pointer;
font-size:12px;
}
</style>
</head>

<body>

<div class="topbar">
<button onclick="addUnit()">➕</button>

<select class="langBtn" onchange="setLang(this)">
<option value="uk">UA</option>
<option value="en">EN</option>
<option value="ru">RU</option>
</select>
</div>

<header>
Divisional Creator<br>
𝐓𝐄𝐑𝐑𝐀 𝐑𝐄𝐆𝐍𝐔𝐌
</header>

<div class="container" id="container"></div>

<script>

let lang="uk";

/* 🌐 UI */
const t={
uk:{
class:"Клас",
unit:"Техніка",
count:"Кількість",
photo:"Фото URL",
units:"одиниць"
},
en:{
class:"Class",
unit:"Vehicle",
count:"Quantity",
photo:"Photo URL",
units:"units"
},
ru:{
class:"Класс",
unit:"Техника",
count:"Количество",
photo:"Ссылка фото",
units:"единиц"
}
};

/* 🧠 ПОВНИЙ СПИСОК КЛАСІВ */
const classNames={
uk:{
OBT:"ОБТ",BTR:"БТР",BMP:"БМП",BMD:"БМД",
BMPT:"БМПТ",SPG:"САУ",ATGM:"ПТРК",
SPAT:"ПТ-САУ",BRDM:"БРДМ",
SAM:"ЗРК",OTRK:"ОТРК",MANPADS:"ПЗРК",
SPAAG:"ЗСУ",

RLS:"РЛС",
FIGHTER:"Винищувач",
ATTACK:"Штурмовик",
BOMBER:"Бомбардувальник",
TRANSPORT_PLANE:"Транспортний літак",
AWACS:"ДРЛО",
INTERCEPTOR:"Перехоплювач",
LIGHT_ATTACK:"ЛВСП",
ATTACK_HELI:"Ударний вертоліт",
TRANSPORT_HELI:"Транспортний вертоліт",

CARRIER:"Авіаносець",
LIGHT_CARRIER:"Малий авіаносець",
FRIGATE:"Фрегат",
DESTROYER:"Есмінець",
CORVETTE:"Корвет",
PATROL:"Патрульний катер",
MISSILE_SHIP:"Ракетоносець",
SUBMARINE:"Підводна лодка",
LCAC:"ДКВП",
MLCAC:"МДКВП",

TRUCK:"Грузова машина"
},

en:{
OBT:"MBT",BTR:"APC",BMP:"IFV",BMD:"Airborne IFV",
BMPT:"BMPT",SPG:"SPG",ATGM:"ATGM",
SPAT:"SP AT",BRDM:"BRDM",
SAM:"SAM",OTRK:"OTRK",MANPADS:"MANPADS",
SPAAG:"SPAAG",

RLS:"Radar",
FIGHTER:"Fighter",
ATTACK:"Attack aircraft",
BOMBER:"Bomber",
TRANSPORT_PLANE:"Transport aircraft",
AWACS:"AWACS",
INTERCEPTOR:"Interceptor",
LIGHT_ATTACK:"Light attack",
ATTACK_HELI:"Attack helicopter",
TRANSPORT_HELI:"Transport helicopter",

CARRIER:"Aircraft carrier",
LIGHT_CARRIER:"Light carrier",
FRIGATE:"Frigate",
DESTROYER:"Destroyer",
CORVETTE:"Corvette",
PATROL:"Patrol boat",
MISSILE_SHIP:"Missile ship",
SUBMARINE:"Submarine",
LCAC:"LCAC",
MLCAC:"Landing craft",

TRUCK:"Truck"
},

ru:{
OBT:"ОБТ",BTR:"БТР",BMP:"БМП",BMD:"БМД",
BMPT:"БМПТ",SPG:"САУ",ATGM:"ПТРК",
SPAT:"ПТ-САУ",BRDM:"БРДМ",
SAM:"ЗРК",OTRK:"ОТРК",MANPADS:"ПЗРК",
SPAAG:"ЗСУ",

RLS:"РЛС",
FIGHTER:"Истребитель",
ATTACK:"Штурмовик",
BOMBER:"Бомбардировщик",
TRANSPORT_PLANE:"Транспортный самолёт",
AWACS:"ДРЛО",
INTERCEPTOR:"Перехватчик",
LIGHT_ATTACK:"ЛВСП",
ATTACK_HELI:"Ударный вертолёт",
TRANSPORT_HELI:"Транспортный вертолёт",

CARRIER:"Авианосец",
LIGHT_CARRIER:"Малый авианосец",
FRIGATE:"Фрегат",
DESTROYER:"Эсминец",
CORVETTE:"Корвет",
PATROL:"Патрульный катер",
MISSILE_SHIP:"Ракетоносец",
SUBMARINE:"Подлодка",
LCAC:"ДКВП",
MLCAC:"МДКВП",

TRUCK:"Грузовая машина"
}
};

/* 🖼 */
function setImage(i){
i.parentElement.querySelector("img").src=i.value;
}

/* 🌐 FIX ПЕРЕКЛАДУ */
function setLang(sel){
lang=sel.value;
updateAll();
}

/* 🔥 ОНОВЛЕННЯ ВСЬОГО */
function updateAll(){

document.querySelectorAll(".classSelect").forEach(sel=>{
let keys=Object.keys(classNames.uk);
sel.innerHTML=
`<option>${t[lang].class}</option>`+
keys.map(k=>`<option value="${k}">${classNames[lang][k]}</option>`).join("");
});

document.querySelectorAll(".unitInput").forEach(i=>i.placeholder=t[lang].unit);
document.querySelectorAll(".photoInput").forEach(i=>i.placeholder=t[lang].photo);
document.querySelectorAll(".countInput").forEach(i=>i.placeholder=t[lang].count);
}

/* 🗑 */
function deleteBlock(btn){
btn.parentElement.remove();
}

/* ➕ */
function addUnit(){

let div=document.createElement("div");
div.className="block";

let keys=Object.keys(classNames.uk);

div.innerHTML=`
<button class="deleteBtn" onclick="deleteBlock(this)">🗑</button>

<img onerror="this.src='https://via.placeholder.com/400x200'">

<select class="classSelect">
<option>${t[lang].class}</option>
${keys.map(k=>`<option value="${k}">${classNames[lang][k]}</option>`).join("")}
</select>

<input class="unitInput" type="text" placeholder="${t[lang].unit}">

<input class="photoInput" type="text" placeholder="${t[lang].photo}" oninput="setImage(this)">

<input class="countInput" type="number" placeholder="${t[lang].count}">
`;

document.getElementById("container").appendChild(div);
}

</script>

</body>
</html>
