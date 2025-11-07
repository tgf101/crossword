# crossword

# html code

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE-EDGE">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title> =Crossword</title>
    <style>
        table{
            border:1px solid rgb(51, 49, 49);
        }
        th{
            width: 50px;;
            height:50px;
            background: rgb(236, 220, 220);
            border:1px solid rgb(241, 227, 227);
            position: relative;
            cursor: grab;
        }

        .b{
            background: black;
            cursor: not-allowed;
        }
        .w{
            background: white;
        }
        .w:hover{

            background: rgb(197, 154, 154);
        }
        th span{
            font-size: 10px;
            position: absolute;
            top:2px;
            left:2px;
        }

        th b{
            font-size: 40px;
            color: green;
        }
        button{
            width: 200px;
            height: 60px;
            margin:15px;
            background: orange;
            cursor: pointer;
            transition: 1s;
        }
        button:hover{
            background: green;
            color: white;
        }
  </style>
</head>
<body>

    <table id="table" cellspacing="0"></table>

    <button onclick="key_check()">CHECK</button>
    <button onclick="color_clear()">WRONG CLEAR</button>
</body>



<script>
    //w- White
    //b - Black
    var values = ["wbwbwwwwwbbbw","wbwbwbwbwbwbw","bbbbwwwwwbbbb","wwwbwwbwwwwww"];
    var ans_key = ["A-B-CDEFG---H","I-J-K-L-M-N-O","----PQRST----","UVW-XY-ZABCDE"];
    var total_rows=values.length;
    var total_cols=values[0].length;
    var spans_value={"0,0":"50","3,5":"60","1,4":"10"};
    var current=null;
    function createFrameBoxes() {
        var boxes="";
        for (let i = 0; i < values.length; i++) { //2
            boxes+="<tr>";
            for (let j = 0; j < values[i].length; j++) {//13
            var s=spans_value[i+","+j]??"";
            boxes+=`<th onclick='myclick(this)' row='${i}' col='${j}' class="${values[i][j]}"><span>${s}</span><b></b></th>`;
            }
            boxes+="</tr>";
        }
        document.getElementById("table").innerHTML=boxes;
    }

    createFrameBoxes();

    function myclick(box) {
        if (box.classList.contains("w")){
            
            var row=box.getAttribute("row")
            var col=box.getAttribute("col")
            if(current!=null){
                current.style.background="transparent";
                
            }

            current=box;
            current.style.background="blue";
        }
    }   



    document.body.onkeyup=function(event){
        if(current!=null){

            //console.log(event);

            if(event.keyCode>=37 && event.keyCode<=40){
            nextmover(event.keyCode);

            }

            if(event.keyCode>=65&&event.keyCode<=90){
                current.querySelector("b").innerHTML=event.key.toUpperCase();
                nextmover(39); //right side
            }

            if(event.keyCode==8||event.keyCode==46){
                current.querySelector("b").innerHTML="";
            }


        }
    }

    function nextmover(code){

            var row=parseInt(current.getAttribute("row"));
            var col=parseInt(current.getAttribute("col"));

            switch (code) {
                case 37: //left:
                    col = col == 0 ? total_cols-1 : col - 1;
                    break;
                case 38: //top:
                    row = row == 0 ? total_rows-1 : row - 1;
                    break;
                case 39: //right:
                    col = col == total_cols-1 ? 0 : col + 1;
                    break;
                case 40: //bottom:
                    row = row == total_rows-1 ? 0 : row + 1;
                    break;
            
                default:
                    break;
            }
            if(current.classList.contains("w")){ //if black then move again
                current.style.background="transparent"
            }
            current=document.querySelectorAll("tr")[row].querySelectorAll("th")[col];

            if(current.classList.contains("b")){ //if black then move again

                nextmover(code);
            }

            else{
                current.style.background="blue"
            }
            
    }

    var red=[];
    var green=[];

    function key_check() {
        
        //only white

        var whites=document.querySelectorAll(".w");
        //console.log(whites);
        whites.forEach(element => {

                var text=element.querySelector("b").innerHTML;
                if(text.length>0){
                    var row=element.getAttribute("row");
                    var col=element.getAttribute("col");
                    console.log(row,col,text,ans_key[row][col]);
                    if(text==ans_key[row][col]){
                        element.style.background="greenyellow";
                        green.push(element);
                    }
                    else{
                        element.style.background="red";
                        red.push(element);
                    }
                }
                
        });

    }

    function color_clear() {
        
        red.forEach(element => {
            element.style.background="transparent";
            element.querySelector("b").innerHTML="";
        });

        green.forEach(element => {
            element.style.background="transparent";
        });

        console.log("WRONG",red);
        console.log("CORRECT",green);

        red.splice(0); //clear red
        green.splice(0); //clear green

    }



</script>
</html>
