    var tttArr=[];
    var matrix = document.getElementsByClassName("box");
    var rib= Math.sqrt(matrix.length);
    var curPlayer;
    var recordGame= [];
    var gamePrevMoves=[];
    var gameFutureMoves=[];
    var curMove={
        imgId: '',
        arrNum: 0,
        cell: [],
        sign: curPlayer
    };
    
    chooseStartPlayer('r');
    reset();
    
     

function reset() {

        cleaning();

        gamePrevMoves=[
            [emptyBoard = { 
            imgId: '',
            arrNum: 0,
            cell: [],
            sign: 'e'    }]
        ];

        gameFutureMoves=[];
        setTable(rib);
}
       
function setTable(num) {
    tttArr=[];
    for (let indexR = 0; indexR < num; indexR++) {
        tttArr.push([]);
        for (let indexC = 0; indexC < num; indexC++) {
            tttArr[indexR][indexC]='e';
         }    
    }
}

function findCurCell(box) {
        
          let curColumn= (+box%rib)-1;
          let curRow = -1;
          if (curColumn<0) {
            curColumn=rib-1;
            curRow = Math.floor(+box/rib)-1;
          }          
          else
            curRow = Math.floor(+box/rib);
          
        return [curRow, curColumn];          
}

function switchPlayer() {
               if (curPlayer=='x') 
                curPlayer='o';
               else
                curPlayer='x';
} 

function recordMove(box) {
        var tempMove={
            imgId : "b" + box,
            arrNum : +box,
            cell : findCurCell(box), 
            sign : curPlayer
        }
        gamePrevMoves.push(tempMove);
        
        return (tempMove);
}

function fill(box) {
     if (tttArr[findCurCell(box)[0]][findCurCell(box)[1]]=='e') {
         curMove = recordMove(box);   
        
//create new img
        var newImg = document.createElement("img");
        newImg.id=curMove.imgId;
        matrix[box-1].appendChild(newImg);
       
            if (curPlayer=='x') 
            {
                document.getElementById(curMove.imgId).src='sources/x.png'  
                tttArr[curMove.cell[0]] [curMove.cell[1]] ='x';
                console.log(tttArr[curMove.cell[0]] [curMove.cell[1]]);
                checkWin();
                switchPlayer();
            }
            
            else
            {
                document.getElementById(curMove.imgId).src='sources/o.png'            
                tttArr[curMove.cell[0]] [curMove.cell[1]]='o';
                console.log(tttArr[curMove.cell[0]] [curMove.cell[1]]);
                checkWin();
                switchPlayer();
            }   
    }   
    
    else
    alert("This cell is already used")    
}

function checkCellEmpty(box) {
            if (matrix[box-1]=='e') {
                return true;
            }
            else
            return false;
}

function stepForward(){
    if(gameFutureMoves.length>0)
    {
        fill(gameFutureMoves[gameFutureMoves.length-1].arrNum);
        gameFutureMoves.pop();
    }     
}

function stepBack(){
    tttArr [gamePrevMoves[gamePrevMoves.length-1].cell[0]] [gamePrevMoves[gamePrevMoves.length-1].cell[1]]='e';
    var imgToDelete = document.getElementById(gamePrevMoves[gamePrevMoves.length-1].imgId);  
    var deleter= (matrix[gamePrevMoves[gamePrevMoves.length-1].arrNum-1].removeChild(imgToDelete));
    gameFutureMoves.push(gamePrevMoves.pop());
    switchPlayer();
}

function deleteCell(cellId, arrIndex) {
    var host = document.getElementById("mainTable");
    var cellToDelete = document.getElementById(cellId);  
    var deleter= (host.removeChild(cellToDelete));
   // var deleter= (host[arrIndex].removeChild(cellToDelete));
    // gamePrevMoves.pop();
}

function cleaning() {
        var index= gamePrevMoves.length-1;   
        while (index>0)
        {
            stepBack();
            index--;
        }
}
    
function printArr() {
     for (let indexA = 0; indexA < tttArr.length; indexA++) {
        for (let indexB = 0; indexB < tttArr[indexA].length; indexB++) {
             console.log(tttArr[indexA][indexB])     
        }      
     }
     console.log("games moves");
     for (let index = 0; index < gamePrevMoves.length; index++) {
        console.log(gamePrevMoves[index]);
         
     }
}

function checkWin() {
        var wonLTR = true;
        var wonRTL = true;
        var wonVertical = true;
        var wonHorizonal = true;
        if ((curMove.cell[0]==curMove.cell[1]) || (curMove.cell[0]+curMove.cell[1]+1==rib)) {           
            //Checking winning in LTR slant
            for (let index = 0; index < rib; index++) {
               if ((tttArr[index][index]==curPlayer) && wonLTR)
               wonLTR=true;
               else
               wonLTR=false;               
            }
            //Checking winning in RTL slant
            for (let index = 0; index < rib; index++) {
                if ((tttArr[index][rib-1-index]==curPlayer) && wonRTL)
                wonRTL=true;
                else
                wonRTL=false;               
             }
        }
        else{
            wonLTR = false;
            wonRTL = false;
        }

        for (let indexA = 0; indexA < rib; indexA++) {
            if ((tttArr[curMove.cell[0]][indexA]==curPlayer)&& wonVertical)
            wonVertical=true;
            else
            wonVertical=false;
            
            if ((tttArr[indexA][curMove.cell[1]]==curPlayer)&& wonHorizonal)
            wonHorizonal=true;
            else
            wonHorizonal=false;
         }
         
         if (wonRTL||wonLTR||wonVertical||wonHorizonal) {
            alert(`THE ${curPlayer} WON! `);
            if(peak(tttArr))
                alert(`THE ${curPlayer} broke a record !  Congratulations!`);
        }
         
}
  
function changeBoardSize(newRib) {
    if(!(newRib>=3))
    {
     do{
        newRib=prompt("enter a number from 3 and up");
        if (newRib==null) {
            newRib=rib;
            break;
        }
    } while ((newRib<3) || (newRib)>(Math.floor(newRib)))   
    }
    rib=+newRib;
    
    creatTable(rib)
    setTable(rib);      
}

function creatTable(num) {
    //creating new cells
        var host = document.getElementById("mainTable");
        reset();
    if (num>Math.sqrt(matrix.length)) {
        for (let index = matrix.length+1; index <= (num*num); index++) {
            var newCell = document.createElement("div");
            newCell.className="box";
            newCell.setAttribute("onclick", `fill('${index}')`);
            newCell.id=`cell${index}`
            host.appendChild(newCell);  
        } 
    }
    else{
        tempNum=matrix.length;
        tempMulti=num*num;
        for (let index = tempNum; index > tempMulti; index--) {
        deleteCell(`cell${index}`, index-1);
        }   
    }
    
    // change CSS respectively
    var newRibPx="";
    for (let index = 0; index < num; index++) {
        newRibPx+="100px ";    
    }
    document.getElementById("mainTable").style.gridTemplateColumns = newRibPx;
    document.getElementById("mainTable").style.gridTemplateRows = newRibPx;
}

function chooseStartPlayer(s) {
    switch (s) {
        case 'O':
        case 'o':
            curPlayer='x';
            switchPlayer();
            break;

        case 'X':
        case 'x':
            curPlayer='o';
            switchPlayer();
            break;
         
        case 'R':
        case 'r':
            if((Math.floor(Math.random() * 10))%2==1)
                curPlayer='x';
            else
                curPlayer='o';
            switchPlayer();
            break;

        case null:
            break;

        default: chooseStartPlayer(prompt("insert x or t or r"));
            break;
    }
}

function hide(params) {
    for (let index = 0; index < matrix.length; index++) 
        matrix[index].hidden=(!(matrix[index].hidden));

        console.log((Math.floor(Math.random() * 10))%2);
}

function peak(newRecordGame) {
    var curGameCounter=0;
    var prevGameCounter=0;
    for (let indexA = 0; indexA < newRecordGame.length; indexA++) {
        for (let indexB = 0; indexB < newRecordGame[indexA].length; indexB++) {
            if(newRecordGame[indexA][indexB]=='e')
            curGameCounter++;   
        }      
     }
     for (let indexA = 0; indexA < recordGame.length; indexA++) {
        for (let indexB = 0; indexB < recordGame[indexA].length; indexB++) {
            if(recordGame[indexA][indexB]=='e')
            prevGameCounter++;   
        }      
     }
     if (curGameCounter>prevGameCounter) {
        recordGame=tttArr;
        //העברת ערכי המטריצה אחד-אחד
        // for (let indexA = 0; indexA < newRecordGame.length; indexA++) {
        //     for (let indexB = 0; indexB < newRecordGame[indexA].length; indexB++) {
        //         recordGame[indexA][indexB]=newRecordGame[indexA][indexB];
                   
        //     }      
        //  }
        return true;
     }
     else
        return false;
}

function loadPeakGame(params) {
    loadGame(recordGame);
}

function loadGame(arr) {
    reset();
    changeBoardSize(Math.sqrt(arr.length));
    var curImgBox=1;
    for (let indexA = 0; indexA < arr.length; indexA++) {
        for (let indexB = 0; indexB < arr[indexA].length; indexB++) {
            document.getElementById( "b" + curImgBox).src=`sources/${ arr[indexA][indexB]}.png`;
            curImgBox++;                
        }      
     }
}
