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
  </style>
</head>
<body>

<table id="table" cellspacing="0"></table>

</body>



<script>
    //w- White
    //b - Black
    var values=["wbwbwwwwwbbbw","wbwbwbwbwbwbw","bbbbwwwwwbbbb","wwwwwwwwwwwww"]
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

            if(event.keyCode>=37 && event.keyCode<=40){
            nextmover(event.keyCode);

            }


        }
    }

    function nextmover(code){

            var row=parseInt(current.getAttribute("row"));
            var col=parseInt(current.getAttribute("col"));

            switch (code) {
                case 37: //left:
                    col = col == 0 ? 12 : col - 1;
                    break;
                case 38: //top:
                    row = row == 0 ? 12 : row - 1;
                    break;
                case 39: //right:
                    col = col == 12 ? 0 : col + 1;
                    break;
                case 40: //bottom:
                    row = row == 12 ? 0 : row + 1;
                    break;
            
                default:
                    break;
            }
            
            current=document.querySelectorAll("tr")[row].querySelectorAll("th")[col];
            current.style.background="blue"
    }

</script>
</html>
