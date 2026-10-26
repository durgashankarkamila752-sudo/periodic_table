# periodic_table<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1">
<title>Neon 3D Periodic Table</title>
<style>
:root {
  --cyan: #00f5ff;
  --purple: #b44dff;
  --bg: #050816;
  --panel: rgba(10, 20, 48, .88);
}
* { box-sizing: border-box; }
body {
  margin: 0;
  color: #eafcff;
  font-family: system-ui, Arial, sans-serif;
  background: radial-gradient(ellipse at 50% 0%, #17245a 0%, #080c20 42%, #02040c 100%);
  min-height: 100vh;
  overflow-x: hidden;
}
#particles {
  position: fixed;
  inset: 0;
  width: 100%;
  height: 100%;
  z-index: -1;
  pointer-events: none;
}
header {
  text-align: center;
  padding: 26px 12px 18px;
}
h1 {
  margin: 0;
  font-size: clamp(25px, 5vw, 48px);
  letter-spacing: 3px;
  color: var(--cyan);
  text-shadow: 0 0 8px var(--cyan), 0 0 25px #007bff, 0 0 45px #8b36ff;
  animation: titlePulse 3s infinite alternate;
}
.subtitle {
  color: #b9caff;
  font-size: 13px;
  letter-spacing: 3px;
  margin-top: 9px;
}
@keyframes titlePulse {
  from { text-shadow: 0 0 7px var(--cyan), 0 0 18px #007bff; }
  to { text-shadow: 0 0 12px var(--cyan), 0 0 30px #7d38ff, 0 0 48px #007bff; }
}
.toolbar {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  gap: 9px;
  padding: 0 12px 20px;
}
input, select, button {
  background: #09152c;
  border: 1px solid #00d9ff;
  color: #eafcff;
  border-radius: 10px;
  padding: 11px 13px;
  font-size: 14px;
}
input { width: min(270px, 80vw); }
button { cursor: pointer; }
button:hover { background: #12365a; box-shadow: 0 0 15px #00d9ff77; }
#status { text-align: center; color: #93faff; font-size: 12px; margin-bottom: 12px; }
.scene {
  perspective: 1100px;
  padding: 10px 12px 25px;
  overflow-x: auto;
  overscroll-behavior-x: contain;
}
.table3d {
  transform-style: preserve-3d;
  animation: hologram 12s ease-in-out infinite alternate;
  transform-origin: center center;
  width: max-content;
  margin: 0 auto;
}
@keyframes hologram {
  0% { transform: rotateX(4deg) rotateY(-2deg); }
  50% { transform: rotateX(-2deg) rotateY(2deg); }
  100% { transform: rotateX(3deg) rotateY(1deg); }
}
.grid {
  display: grid;
  grid-template-columns: repeat(18, minmax(45px, 58px));
  grid-template-rows: repeat(7, 67px);
  gap: 5px;
}
.element {
  position: relative;
  min-width: 0;
  border: 1px solid var(--color, #00f5ff);
  border-radius: 7px;
  background: linear-gradient(145deg, #12213bcf, #070d20f2);
  box-shadow: inset 0 0 9px color-mix(in srgb, var(--color, #00f5ff) 18%, transparent),
              0 0 7px color-mix(in srgb, var(--color, #00f5ff) 32%, transparent);
  text-align: center;
  padding: 4px 1px;
  cursor: pointer;
  transition: transform .2s, box-shadow .2s, opacity .2s;
  transform-style: preserve-3d;
  animation: cardGlow 3.5s infinite alternate;
  color: #fff;
}
@keyframes cardGlow {
  from { box-shadow: 0 0 4px color-mix(in srgb, var(--color) 30%, transparent); }
  to { box-shadow: 0 0 12px color-mix(in srgb, var(--color) 55%, transparent); }
}
.element:hover, .element:focus-visible {
  z-index: 5;
  transform: translateZ(24px) scale(1.12);
  box-shadow: 0 0 20px var(--color), inset 0 0 15px var(--color);
  outline: none;
}
.element .num { display: block; font-size: 9px; text-align: left; padding-left: 3px; opacity: .85; }
.element .sym {
  display: block; font-weight: 800; font-size: 21px;
  line-height: 27px; color: var(--color);
  text-shadow: 0 0 8px var(--color);
}
.element .name {
  display: block; font-size: 8px; white-space: nowrap;
  overflow: hidden; text-overflow: ellipsis;
}
.element.dimmed { opacity: .13; filter: grayscale(1); }
.element.selected { background: #19385a; outline: 2px solid white; }
.series {
  display: grid;
  grid-template-columns: repeat(15, minmax(45px, 58px));
  gap: 5px;
  width: max-content;
  margin: 13px auto 0;
}
.series-label {
  grid-column: 1 / -1;
  color: #c5d5ff;
  font-size: 12px;
  letter-spacing: 2px;
  padding: 8px 0 2px;
}
.legend {
  display: flex; flex-wrap: wrap; justify-content: center;
  gap: 8px 14px; padding: 8px 12px 22px;
  max-width: 1000px; margin: auto;
}
.legend span { font-size: 11px; color: #d4e6ff; }
.dot {
  display: inline-block; width: 9px; height: 9px;
  border-radius: 3px; margin-right: 5px;
  box-shadow: 0 0 8px currentColor;
}
.info {
  width: min(750px, calc(100% - 24px));
  margin: 0 auto 28px;
  padding: 18px;
  background: var(--panel);
  border: 1px solid #00d9ff99;
  border-radius: 17px;
  box-shadow: 0 0 25px #006aff24, inset 0 0 18px #00d9ff0b;
}
.info h2 { color: var(--cyan); margin: 0 0 8px; }
#detailSymbol {
  float: right; font-size: 52px; font-weight: 900;
  color: var(--purple); text-shadow: 0 0 20px var(--purple);
}
.details {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(130px, 1fr));
  gap: 9px; margin-top: 15px;
}
.detail {
  border: 1px solid #405b88;
  background: #101c35; border-radius: 8px; padding: 10px;
}
.detail small { display: block; color: #91a9d7; margin-bottom: 4px; }
footer { text-align: center; color: #7185b4; font-size: 11px; padding: 0 10px 24px; }
@media (max-width: 600px) {
  header { padding-top: 22px; }
  .scene { padding-left: 10px; padding-right: 10px; }
  .grid { grid-template-columns: repeat(18, 45px); grid-template-rows: repeat(7, 60px); gap: 4px; }
  .series { grid-template-columns: repeat(15, 45px); gap: 4px; }
  .element .sym { font-size: 18px; line-height: 24px; }
  .element .name { font-size: 7px; }
  .table3d { animation-duration: 16s; }
}
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after { animation-duration: .01ms !important; transition-duration: .01ms !important; }
}
</style>
</head>
<body>
<canvas id="particles"></canvas>

<header>
  <h1>NEON PERIODIC</h1>
  <div class="subtitle">3D HOLOGRAPHIC ELEMENT EXPLORER</div>
</header>

<div class="toolbar">
  <input id="search" type="search" placeholder="🔍 Search element, symbol or number..." aria-label="Search elements">
  <select id="category" aria-label="Filter element category">
    <option value="all">All elements</option>
    <option value="alkali">Alkali metals</option>
    <option value="alkaline">Alkaline earth metals</option>
    <option value="transition">Transition metals</option>
    <option value="post">Post-transition metals</option>
    <option value="metalloid">Metalloids</option>
    <option value="nonmetal">Other nonmetals</option>
    <option value="halogen">Halogens</option>
    <option value="noble">Noble gases</option>
    <option value="lanthanoid">Lanthanides</option>
    <option value="actinoid">Actinides</option>
  </select>
  <button id="motionBtn">⏸ Pause motion</button>
</div>
<div id="status">118 ELEMENTS · TAP ANY TILE TO EXPLORE</div>

<div class="scene">
  <div class="table3d" id="table3d">
    <div class="grid" id="mainGrid"></div>
    <div class="series" id="lanGrid"><div class="series-label">LANTHANIDES · 57–71</div></div>
    <div class="series" id="actGrid"><div class="series-label">ACTINIDES · 89–103</div></div>
  </div>
</div>

<div class="legend" id="legend"></div>

<section class="info" aria-live="polite">
  <div id="detailSymbol">H</div>
  <h2 id="detailName">Hydrogen</h2>
  <div id="detailDescription">The lightest chemical element.</div>
  <div class="details">
    <div class="detail"><small>Atomic Number</small><strong id="dNum">1</strong></div>
    <div class="detail"><small>Atomic Mass (u)</small><strong id="dMass">1.008</strong></div>
    <div class="detail"><small>Category</small><strong id="dCat">Other nonmetal</strong></div>
    <div class="detail"><small>Electron Configuration</small><strong id="dConfig">1s¹</strong></div>
    <div class="detail"><small>Period</small><strong id="dPeriod">1</strong></div>
    <div class="detail"><small>Group</small><strong id="dGroup">1</strong></div>
    <div class="detail"><small>State at ~25°C</small><strong id="dState">Gas</strong></div>
    <div class="detail"><small>Discovered by</small><strong id="dDiscoverer">Henry Cavendish</strong></div>
  </div>
</section>
<footer>NEON PERIODIC · Interactive chemistry learning experience · Atomic masses are approximate standard values.</footer>

<script>
/* Atomic number | Symbol | Name | Mass | Category | Period | Group
   Position of lanthanides/actinides is shown in separate rows. */
const raw = `
1|H|Hydrogen|1.008|nonmetal|1|1
2|He|Helium|4.0026|noble|1|18
3|Li|Lithium|6.94|alkali|2|1
4|Be|Beryllium|9.0122|alkaline|2|2
5|B|Boron|10.81|metalloid|2|13
6|C|Carbon|12.011|nonmetal|2|14
7|N|Nitrogen|14.007|nonmetal|2|15
8|O|Oxygen|15.999|nonmetal|2|16
9|F|Fluorine|18.998|halogen|2|17
10|Ne|Neon|20.180|noble|2|18
11|Na|Sodium|22.990|alkali|3|1
12|Mg|Magnesium|24.305|alkaline|3|2
13|Al|Aluminium|26.982|post|3|13
14|Si|Silicon|28.085|metalloid|3|14
15|P|Phosphorus|30.974|nonmetal|3|15
16|S|Sulfur|32.06|nonmetal|3|16
17|Cl|Chlorine|35.45|halogen|3|17
18|Ar|Argon|39.948|noble|3|18
19|K|Potassium|39.098|alkali|4|1
20|Ca|Calcium|40.078|alkaline|4|2
21|Sc|Scandium|44.956|transition|4|3
22|Ti|Titanium|47.867|transition|4|4
23|V|Vanadium|50.942|transition|4|5
24|Cr|Chromium|51.996|transition|4|6
25|Mn|Manganese|54.938|transition|4|7
26|Fe|Iron|55.845|transition|4|8
27|Co|Cobalt|58.933|transition|4|9
28|Ni|Nickel|58.693|transition|4|10
29|Cu|Copper|63.546|transition|4|11
30|Zn|Zinc|65.38|transition|4|12
31|Ga|Gallium|69.723|post|4|13
32|Ge|Germanium|72.630|metalloid|4|14
33|As|Arsenic|74.922|metalloid|4|15
34|Se|Selenium|78.971|nonmetal|4|16
35|Br|Bromine|79.904|halogen|4|17
36|Kr|Krypton|83.798|noble|4|18
37|Rb|Rubidium|85.468|alkali|5|1
38|Sr|Strontium|87.62|alkaline|5|2
39|Y|Yttrium|88.906|transition|5|3
40|Zr|Zirconium|91.224|transition|5|4
41|Nb|Niobium|92.906|transition|5|5
42|Mo|Molybdenum|95.95|transition|5|6
43|Tc|Technetium|98|transition|5|7
44|Ru|Ruthenium|101.07|transition|5|8
45|Rh|Rhodium|102.906|transition|5|9
46|Pd|Palladium|106.42|transition|5|10
47|Ag|Silver|107.868|transition|5|11
48|Cd|Cadmium|112.414|transition|5|12
49|In|Indium|114.818|post|5|13
50|Sn|Tin|118.710|post|5|14
51|Sb|Antimony|121.760|metalloid|5|15
52|Te|Tellurium|127.60|metalloid|5|16
53|I|Iodine|126.904|halogen|5|17
54|Xe|Xenon|131.293|noble|5|18
55|Cs|Caesium|132.905|alkali|6|1
56|Ba|Barium|137.327|alkaline|6|2
57|La|Lanthanum|138.905|lanthanoid|6|3
58|Ce|Cerium|140.116|lanthanoid|6|4
59|Pr|Praseodymium|140.908|lanthanoid|6|5
60|Nd|Neodymium|144.242|lanthanoid|6|6
61|Pm|Promethium|145|lanthanoid|6|7
62|Sm|Samarium|150.36|lanthanoid|6|8
63|Eu|Europium|151.964|lanthanoid|6|9
64|Gd|Gadolinium|157.25|lanthanoid|6|10
65|Tb|Terbium|158.925|lanthanoid|6|11
66|Dy|Dysprosium|162.500|lanthanoid|6|12
67|Ho|Holmium|164.930|lanthanoid|6|13
68|Er|Erbium|167.259|lanthanoid|6|14
69|Tm|Thulium|168.934|lanthanoid|6|15
70|Yb|Ytterbium|173.045|lanthanoid|6|16
71|Lu|Lutetium|174.967|lanthanoid|6|17
72|Hf|Hafnium|178.49|transition|6|4
73|Ta|Tantalum|180.948|transition|6|5
74|W|Tungsten|183.84|transition|6|6
75|Re|Rhenium|186.207|transition|6|7
76|Os|Osmium|190.23|transition|6|8
77|Ir|Iridium|192.217|transition|6|9
78|Pt|Platinum|195.084|transition|6|10
79|Au|Gold|196.967|transition|6|11
80|Hg|Mercury|200.592|transition|6|12
81|Tl|Thallium|204.38|post|6|13
82|Pb|Lead|207.2|post|6|14
83|Bi|Bismuth|208.980|post|6|15
84|Po|Polonium|209|post|6|16
85|At|Astatine|210|halogen|6|17
86|Rn|Radon|222|noble|6|18
87|Fr|Francium|223|alkali|7|1
88|Ra|Radium|226|alkaline|7|2
89|Ac|Actinium|227|actinoid|7|3
90|Th|Thorium|232.038|actinoid|7|4
91|Pa|Protactinium|231.036|actinoid|7|5
92|U|Uranium|238.029|actinoid|7|6
93|Np|Neptunium|237|actinoid|7|7
94|Pu|Plutonium|244|actinoid|7|8
95|Am|Americium|243|actinoid|7|9
96|Cm|Curium|247|actinoid|7|10
97|Bk|Berkelium|247|actinoid|7|11
98|Cf|Californium|251|actinoid|7|12
99|Es|Einsteinium|252|actinoid|7|13
100|Fm|Fermium|257|actinoid|7|14
101|Md|Mendelevium|258|actinoid|7|15
102|No|Nobelium|259|actinoid|7|16
103|Lr|Lawrencium|266|actinoid|7|17
104|Rf|Rutherfordium|267|transition|7|4
105|Db|Dubnium|268|transition|7|5
106|Sg|Seaborgium|269|transition|7|6
107|Bh|Bohrium|270|transition|7|7
108|Hs|Hassium|277|transition|7|8
109|Mt|Meitnerium|278|transition|7|9
110|Ds|Darmstadtium|281|transition|7|10
111|Rg|Roentgenium|282|transition|7|11
112|Cn|Copernicium|285|transition|7|12
113|Nh|Nihonium|286|post|7|13
114|Fl|Flerovium|289|post|7|14
115|Mc|Moscovium|290|post|7|15
116|Lv|Livermorium|293|post|7|16
117|Ts|Tennessine|294|halogen|7|17
118|Og|Oganesson|294|noble|7|18
`.trim();

const categories = {
  alkali: {name:"Alkali metal", color:"#ff5d91"},
  alkaline: {name:"Alkaline earth metal", color:"#ffb454"},
  transition: {name:"Transition metal", color:"#54d6ff"},
  post: {name:"Post-transition metal", color:"#b4a0ff"},
  metalloid: {name:"Metalloid", color:"#61ffb1"},
  nonmetal: {name:"Other nonmetal", color:"#00f5ff"},
  halogen: {name:"Halogen", color:"#ffe66d"},
  noble: {name:"Noble gas", color:"#ff80ed"},
  lanthanoid: {name:"Lanthanide", color:"#7dffed"},
  actinoid: {name:"Actinide", color:"#ff9b70"}
};
const elements = raw.split("\n").map(line => {
  const [n,sym,name,mass,cat,period,group] = line.split("|");
  return {n:+n,sym,name,mass,cat,period:+period,group:+group};
});
const special = {
  H:["1s¹","Gas","Henry Cavendish","The lightest element; important in stars and water."],
  He:["1s²","Gas","Pierre Janssen and Norman Lockyer","A noble gas used in balloons and cryogenic systems."],
  C:["[He] 2s² 2p²","Solid","Known since antiquity","The basis of organic chemistry and all known life."],
  O:["[He] 2s² 2p⁴","Gas","Carl Wilhelm Scheele and Joseph Priestley","Essential for respiration and combustion."],
  Na:["[Ne] 3s¹","Solid","Humphry Davy","A reactive alkali metal found in common salt compounds."],
  Fe:["[Ar] 3d⁶ 4s²","Solid","Known since antiquity","A major component of steel and Earth's core."],
  Cu:["[Ar] 3d¹⁰ 4s¹","Solid","Known since antiquity","A highly useful conductor of electricity."],
  Ag:["[Kr] 4d¹⁰ 5s¹","Solid","Known since antiquity","A precious metal with excellent electrical conductivity."],
  Au:["[Xe] 4f¹⁴ 5d¹⁰ 6s¹","Solid","Known since antiquity","A corrosion-resistant precious metal used in electronics and jewellery."],
  Hg:["[Xe] 4f¹⁴ 5d¹⁰ 6s²","Liquid","Known since antiquity","A metal that is liquid at room temperature; toxic."],
  U:["[Rn] 5f³ 6d¹ 7s²","Solid","Martin Heinrich Klaproth","A radioactive element used as nuclear fuel."]
};
function configFor(e) {
  if (special[e.sym]) return special[e.sym][0];
  const configs = {
    Li:"[He] 2s¹", Be:"[He] 2s²", B:"[He] 2s² 2p¹",
    N:"[He] 2s² 2p³", F:"[He] 2s² 2p⁵", Ne:"[He] 2s² 2p⁶",
    Mg:"[Ne] 3s²", Al:"[Ne] 3s² 3p¹", Si:"[Ne] 3s² 3p²",
    P:"[Ne] 3s² 3p³", S:"[Ne] 3s² 3p⁴", Cl:"[Ne] 3s² 3p⁵",
    Ar:"[Ne] 3s² 3p⁶", K:"[Ar] 4s¹", Ca:"[Ar] 4s²"
  };
  return configs[e.sym] || "See reference data";
}
function stateFor(e) {
  if (["Hg","Br"].includes(e.sym)) return "Liquid";
  if (["H","He","N","O","F","Ne","Cl","Ar","Kr","Xe","Rn"].includes(e.sym)) return "Gas";
  if (["Og","Ts","Lv","Mc","Fl","Nh","Cn","Rg","Ds","Mt","Hs","Bh","Sg","Db","Rf"].includes(e.sym)) return "Predicted";
  return "Solid";
}
function discovererFor(e) {
  if (special[e.sym]) return special[e.sym][2];
  if (e.n > 103) return "International research teams";
  if (e.n > 92) return "Various research teams";
  if (e.n > 83) return "Various scientists";
  return "Known since antiquity / historical discovery";
}
function descriptionFor(e) {
  if (special[e.sym]) return special[e.sym][3];
  return `${e.name} is classified as a ${categories[e.cat].name.toLowerCase()}. Its chemical behaviour depends on its atomic structure and electrons.`;
}
const mainGrid = document.getElementById("mainGrid");
const lanGrid = document.getElementById("lanGrid");
const actGrid = document.getElementById("actGrid");
const legend = document.getElementById("legend");
let selected = elements[0];
let paused = false;

function makeTile(e, target, col, row) {
  const b = document.createElement("button");
  b.className = "element";
  b.type = "button";
  b.style.setProperty("--color", categories[e.cat].color);
  if (target === mainGrid) {
    b.style.gridColumn = col;
    b.style.gridRow = row;
  } else {
    b.style.gridColumn = col;
  }
  b.dataset.n = e.n;
  b.dataset.name = e.name.toLowerCase();
  b.dataset.sym = e.sym.toLowerCase();
  b.dataset.cat = e.cat;
  b.innerHTML = `<span class="num">${e.n}</span><span class="sym">${e.sym}</span><span class="name">${e.name}</span>`;
  b.setAttribute("aria-label", `${e.name}, atomic number ${e.n}`);
  b.addEventListener("click", () => showElement(e, b));
  target.appendChild(b);
}
elements.forEach(e => {
  if (e.cat === "lanthanoid") {
    makeTile(e, lanGrid, e.n - 56, 1);
  } else if (e.cat === "actinoid") {
    makeTile(e, actGrid, e.n - 88, 1);
  } else {
    makeTile(e, mainGrid, e.group, e.period);
  }
});
Object.entries(categories).forEach(([key, value]) => {
  const span = document.createElement("span");
  span.innerHTML = `<i class="dot" style="background:${value.color};color:${value.color}"></i>${value.name}`;
  legend.appendChild(span);
});
function showElement(e, tile) {
  selected = e;
  document.querySelectorAll(".element").forEach(el => el.classList.remove("selected"));
  if (tile) tile.classList.add("selected");
  document.getElementById("detailSymbol").textContent = e.sym;
  document.getElementById("detailName").textContent = `${e.name} (${e.sym})`;
  document.getElementById("detailDescription").textContent = descriptionFor(e);
  document.getElementById("dNum").textContent = e.n;
  document.getElementById("dMass").textContent = e.mass;
  document.getElementById("dCat").textContent = categories[e.cat].name;
  document.getElementById("dConfig").textContent = configFor(e);
  document.getElementById("dPeriod").textContent = e.period;
  document.getElementById("dGroup").textContent = e.cat === "lanthanoid" || e.cat === "actinoid" ? "f-block" : e.group;
  document.getElementById("dState").textContent = stateFor(e);
  document.getElementById("dDiscoverer").textContent = discovererFor(e);
}
showElement(elements[0], mainGrid.querySelector('[data-n="1"]'));

function filterElements() {
  const q = document.getElementById("search").value.trim().toLowerCase();
  const cat = document.getElementById("category").value;
  let count = 0;
  document.querySelectorAll(".element").forEach(tile => {
    const matchQ = !q || tile.dataset.name.includes(q) || tile.dataset.sym.includes(q) || tile.dataset.n === q;
    const matchCat = cat === "all" || tile.dataset.cat === cat;
    const match = matchQ && matchCat;
    tile.classList.toggle("dimmed", !match);
    if (match) count++;
  });
  document.getElementById("status").textContent = `${count} OF 118 ELEMENTS MATCH · TAP A TILE TO EXPLORE`;
}
document.getElementById("search").addEventListener("input", filterElements);
document.getElementById("category").addEventListener("change", filterElements);
document.getElementById("motionBtn").addEventListener("click", () => {
  paused = !paused;
  document.getElementById("table3d").style.animationPlayState = paused ? "paused" : "running";
  document.querySelectorAll(".element").forEach(t => t.style.animationPlayState = paused ? "paused" : "running");
  document.getElementById("motionBtn").textContent = paused ? "▶ Resume motion" : "⏸ Pause motion";
});

/* Floating neon particles */
const canvas = document.getElementById("particles");
const ctx = canvas.getContext("2d");
let w, h, particles = [];
function resizeCanvas() {
  const dpr = Math.min(window.devicePixelRatio || 1, 2);
  w = window.innerWidth; h = window.innerHeight;
  canvas.width = w * dpr; canvas.height = h * dpr;
  canvas.style.width = w + "px"; canvas.style.height = h + "px";
  ctx.setTransform(dpr, 0, 0, dpr, 0, 0);
  particles = Array.from({length: Math.min(75, Math.floor(w / 6))}, () => ({
    x: Math.random()*w, y: Math.random()*h,
    r: Math.random()*2+0.5, vy: Math.random()*0.5+0.12,
    vx: (Math.random()-.5)*.35, a: Math.random()*.6+.2,
    hue: Math.random() > .5 ? 185 : 275
  }));
}
function animateParticles() {
  ctx.clearRect(0, 0, w, h);
  for (const p of particles) {
    p.x += p.vx; p.y -= p.vy;
    if (p.y < -5) { p.y = h + 5; p.x = Math.random()*w; }
    if (p.x < -5) p.x = w+5;
    if (p.x > w+5) p.x = -5;
    ctx.beginPath();
    ctx.fillStyle = `hsla(${p.hue},100%,70%,${p.a})`;
    ctx.shadowColor = `hsl(${p.hue},100%,65%)`;
    ctx.shadowBlur = 12;
    ctx.arc(p.x,p.y,p.r,0,Math.PI*2);
    ctx.fill();
  }
  ctx.shadowBlur = 0;
  requestAnimationFrame(animateParticles);
}
window.addEventListener("resize", resizeCanvas);
resizeCanvas();
animateParticles();
</script>
</body>
</html>
