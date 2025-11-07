# crossword

## HTML, CSS, AND JAVASCRIPT 
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Crossword Puzzle</title>
<style>
body { font-family: Arial, sans-serif; margin:0; }
table { border:1px solid #333; border-collapse: collapse; margin:10px auto; table-layout: fixed; }
th { width:35px; height:35px; border:1px solid #aaa; position: relative; text-align:center; vertical-align:middle; font-size:16px; padding:0; cursor:pointer;}
.black { background:black; cursor:not-allowed; }
.white { background:white; }
.white:hover { background:#c59a9a; }
th span { font-size:8px; position:absolute; top:1px; left:1px; color:black; }
th b { font-size:16px; color:green; }
button { width:200px; height:50px; margin:10px; background:orange; cursor:pointer; transition:0.5s; }
button:hover { background:green; color:white; }
.clues { display:flex; justify-content: space-between; margin:10px; }
.clues div { width:45%; }
.clues h3 { margin-bottom:5px; }
</style>
</head>
<body>

<h2 style="text-align:center;">Crossword Puzzle</h2>

<table id="table"></table>

<div style="text-align:center;">
<button onclick="key_check()">CHECK</button>
<button onclick="color_clear()">WRONG CLEAR</button>
</div>

<div class="clues">
<div>
<h3>Across</h3>
<ul>
<li>1. Color of snow (5)</li>
<li>2. Opposite of dark (3)</li>
<li>3. Vast space above (3)</li>
<li>4. Grows in forests (4)</li>
<li>5. Place to live (5)</li>
<li>6. Vehicle on road (3)</li>
<li>7. Blooms in garden (6)</li>
<li>8. Visible at night in the sky (4)</li>
<li>9. Writing tool (3)</li>
<li>10. Man's best friend (3)</li>
<li>11. Small object that opens doors (3)</li>
<li>12. Money piece (4)</li>
<li>13. Liquid essential for life (5)</li>
<li>14. Twinkles in the night sky (4)</li>
<li>15. Flies in the air (5)</li>
<li>17. Precipitation in winter (4)</li>
<li>18. Falls from clouds (3)</li>
<li>19. Large body of water (3)</li>
<li>20. Compass direction (4)</li>
<li>21. Swims in water (5)</li>
<li>22. Metal fastener (4)</li>
<li>23. Shines bright (5)</li>
</ul>
</div>
<div>
<h3>Down</h3>
<ul>
<li>1. Vehicle on tracks (5)</li>
<li>2. Reads in library (4)</li>
<li>3. Pet that purrs (3)</li>
<li>4. A type of flower (4)</li>
</ul>
</div>
</div>

<script>
// 'b' = black cell, 'w' = white cell
var values = [
"wbwwwwwwwwwwb",
"wbbbwbwbwbbbw",
"wbwwwbwbwwwww",
"wbwbbbwbwbbbw",
"wwwwbwwwwbwww",
"wbwbwbbbwbbbw",
"bbwbwwwwwbwbb",
"wbbbwbbbwbwbw",
"wwwbwwwwbwwww",
"wbbbwbwbbbwbw",
"wwwwwbwbwwwbw",
"wbbbwbwbwbbbw",
"bwwwwwwwwwwbw"
];

// Answers mapped to numbers ("" = empty cell for player to fill)
var ans_key = [
["W","","H","I","T","E","","B","L","A","C","K",""],
["A","","","","T","","R","","E","","E","",""],
["T","R","A","I","N","","F","","L","O","W","E","R"],
["E","","S","","","","C","","A","R","","",""],
["R","O","S","E","","F","L","O","W","E","R","",""],
["","","S","U","N","","","","M","O","O","N","",""],
["B","O","O","K","","","","P","E","N","","",""],
["","","C","A","T","","","","D","O","G","","",""],
["H","O","U","S","E","","","","K","E","Y","",""],
["","","M","A","P","","","","C","O","I","N","",""],
["F","I","S","H","","","","W","A","T","E","R","",""],
["","","","S","T","A","R","","","","P","L","A","N","E"],
["S","N","O","W","","","","R","A","I","N","",""]
];

// Numbered clues
var spans_value = {"0,0":"1","0,2":"2","0,4":"3","0,6":"4","0,8":"5","1,12":"6","2,2":"7","2,8":"8","4,0":"9","4,5":"10","4,10":"11","5,12":"12","6,3":"13","6,9":"14","7,0":"15","8,0":"17","8,4":"18","8,6":"19","8,9":"20","10,0":"21","10,8":"22","12,1":"23"};

var total_rows = values.length;
var total_cols = values[0].length;
var current = null;
var red=[], green=[];

// Build crossword table
function createFrameBoxes() {
    var boxes="";
    for(let i=0;i<total_rows;i++){
        boxes+="<tr>";
        for(let j=0;j<total_cols;j++){
            var cls = values[i][j]==="b"?"black":"white";
            var num = spans_value[i+","+j]??"";
            boxes+=`<th onclick="myclick(this)" row="${i}" col="${j}" class="${cls}"><span>${num}</span><b></b></th>`;
        }
        boxes+="</tr>";
    }
    document.getElementById("table").innerHTML = boxes;
}
createFrameBoxes();

// Click to select white cell
function myclick(box){
    if(box.classList.contains("white")){
        if(current) current.style.background="white";
        current = box;
        current.style.background="blue";
    }
}

// Keyboard input
document.body.onkeyup=function(event){
    if(!current || !current.classList.contains("white")) return;
    if(event.keyCode>=65 && event.keyCode<=90){
        current.querySelector("b").innerHTML = event.key.toUpperCase();
        moveNext(39);
    }
    if(event.keyCode==8 || event.keyCode==46){
        current.querySelector("b").innerHTML="";
    }
    if(event.keyCode>=37 && event.keyCode<=40) moveNext(event.keyCode);
}

// Move cursor skipping black cells
function moveNext(code){
    var row=parseInt(current.getAttribute("row"));
    var col=parseInt(current.getAttribute("col"));
    switch(code){
        case 37: col=col==0?total_cols-1:col-1; break;
        case 38: row=row==0?total_rows-1:row-1; break;
        case 39: col=col==total_cols-1?0:col+1; break;
        case 40: row=row==total_rows-1?0:row+1; break;
    }
    var nextCell = document.querySelectorAll("tr")[row].querySelectorAll("th")[col];
    if(nextCell.classList.contains("black")) {
        current.style.background="white";
        current = nextCell;
        moveNext(code);
    } else {
        current.style.background="white";
        current = nextCell;
        current.style.background="blue";
    }
}

// Check answers
function key_check(){
    red=[]; green=[];
    var whites=document.querySelectorAll(".white");
    whites.forEach(el=>{
        var row=parseInt(el.getAttribute("row"));
        var col=parseInt(el.getAttribute("col"));
        var val=el.querySelector("b").innerHTML;
        if(val && ans_key[row][col]===val) green.push(el);
        else if(val && val!=="") red.push(el);
    });
    red.forEach(el=>el.style.background="red");
    green.forEach(el=>el.style.background="greenyellow");
    if(green.length===whites.length) alert("🎉 All correct!");
}

// Clear wrong cells
function color_clear(){
    red.forEach(el=>{el.style.background="white"; el.querySelector("b").innerHTML="";});
    green.forEach(el=>{el.style.background="white";});
    red=[]; green=[];
}
</script>
</body>
</html>
