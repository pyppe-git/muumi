<html lang="fi">
<head>
<meta charset="UTF-8">
<title>Muumihahmojen keräily</title>
<style>
  body { font-family: Arial; text-align: center; }
  button { padding: 10px 20px; margin: 5px; }
  #log { margin-top: 20px; max-height: 200px; overflow-y: auto; border: 1px solid #ccc; padding: 5px; }
  .character { display: inline-block; margin: 5px; text-align: center; }
  .character img { width: 60px; height: 60px; display: block; margin-bottom: 3px; }
</style>
</head>
<body>

<h2>Muumihahmojen keräily</h2>

<button onclick="reset()">Aloita alusta</button>
<button onclick="draw()">Ota muna</button>
<button onclick="autoDraw()">Osta kunnes kaikki kasassa</button>

<h3 id="status"></h3>
<h3 id="money"></h3>

<h4>Kerätyt hahmot:</h4>
<div id="collectedList"></div>

<h4>Puuttuvat hahmot:</h4>
<div id="missingList"></div>

<div id="log"></div>

<script>
const characters = [
  {name:"Muumipeikko", img:"muumipeikko.png"},
  {name:"Niiskuneiti", img:"niiskuneiti.png"},
  {name:"Muumimamma", img:"muumimamma.png"},
  {name:"Muumipappa", img:"muumipappa.png"},
  {name:"Haisuli", img:"haisuli.png"},
  {name:"Mörkö", img:"morko.png"},
  {name:"Hemuli", img:"hemuli.png"},
  {name:"Vilijonkka", img:"vilijonkka.png"},
  {name:"Pikku Myy", img:"pikku_myy.png"},
  {name:"Nipsu", img:"nipsu.png"},
  {name:"Hattivatti", img:"hattivatti.png"},
  {name:"Nuuskamuikkunen", img:"nuuskamuikkunen.png"}
];

let collected = new Set();
let draws = 0;
const pricePerEgg = 3.25;

function reset() {
    collected.clear();
    draws = 0;
    document.getElementById("log").innerHTML = "";
    updateStatus();
}

function draw() {
    const index = Math.floor(Math.random() * characters.length);
    const figure = characters[index].name;

    draws++;
    collected.add(figure);

    const log = document.getElementById("log");
    log.innerHTML = "Muna " + draws + ": " + figure + "<br>" + log.innerHTML;

    updateStatus();
}

function updateStatus() {
    document.getElementById("status").innerText =
        "Kerätty: " + collected.size + " / " + characters.length +
        " | Munia: " + draws;

    const totalCost = (draws * pricePerEgg).toFixed(2);
    document.getElementById("money").innerText =
        "Rahaa kulunut: " + totalCost + " €";

    // Kerätyt
    const collectedDiv = document.getElementById("collectedList");
    collectedDiv.innerHTML = "";
    characters.forEach(c => {
        if(collected.has(c.name)){
            collectedDiv.innerHTML += `<div class="character"><img src="${c.img}" alt="${c.name}">${c.name}</div>`;
        }
    });

    // Puuttuvat
    const missingDiv = document.getElementById("missingList");
    missingDiv.innerHTML = "";
    characters.forEach(c => {
        if(!collected.has(c.name)){
            missingDiv.innerHTML += `<div class="character"><img src="${c.img}" alt="${c.name}">${c.name}</div>`;
        }
    });
}

// Automaattinen ostaminen, kunnes kaikki hahmot kasassa
function autoDraw() {
    while (collected.size < characters.length) {
        draw();
    }
}

reset();
</script>

</body>
</html>
