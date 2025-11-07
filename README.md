# crossword

# html code

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE-EDGE">
    <meta name="viewport" content="width=device-width. initial-scale=1.0">
    <title> =Crossword</title>
    <style>
        table{
            border:1px solid red;
        }
        th{
            width: 50px;;
            height:50px;
            background: rgb(236, 220, 220);
            border:1px solid rgb(110, 105, 105);
            position: relative;
        }

        .b{
            background: black;
        }
        .w{
            background: white;
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
    var spans_value=[[5,8,50]];

    function createFrameBoxes() {
        var boxes="";
        for (let i = 0; i < values.length; i++) { //2
            boxes+="<tr>";
            for (let j = 0; j < values[i].length; j++) {//13
            boxes+=`<th class="${values[i][j]}"><span></span><b></b></th>`;
            }
            boxes+="</tr>";
        }
        document.getElementById("table").innerHTML=boxes;
    }

    createFrameBoxes();
</script>
</html>
