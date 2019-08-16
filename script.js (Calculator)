// global variables - the in-code equation reference and the index that the program is working with
var equation = []
var currentIndex = -1

window.onload = function() {
  // assign all of the number buttons their onclick
  let nums = document.getElementsByClassName("number");
  for (var i = 0; i < nums.length; i++) {
    nums[i].onclick = numClicked
  }

  // assign all of the basic operations buttons their onclick
  let basicOps = document.getElementsByClassName("basic-operation")
  for (var i = 0; i < basicOps.length; i++) {
    basicOps[i].onclick = basicOpClicked
  }

  // assign both the parentheses buttons their onclick
  let parens = document.getElementsByClassName("parentheses")
  for (var i = 0; i < parens.length; i++) {
    parens[i].onclick = parenClicked
  }

  // assign both the index mover buttons their onclick
  let iMovers = document.getElementsByClassName("index-mover")
  for (var i = 0; i < iMovers.length; i++) {
    iMovers[i].onclick = iMoverClicked
  }

  // assign special buttons their onclick operations
  document.getElementById("clear").onclick = clearClicked
  document.getElementById("delete").onclick = deleteClicked
  document.getElementById("equals").onclick = equalsClicked
}

function addToDisplay(toAdd) {
  // move the working index forwards
  currentIndex++
  indexForward()
  // for reference purposes
  old = document.getElementById("display").innerHTML
  // figure out and assign new string
  document.getElementById("display").innerHTML = old.substr(0, currentIndex) + toAdd + old.substr(currentIndex + 1)
  // update the in-code equation
  equation[currentIndex] = toAdd
}

var numClicked = function() {
  // numbers are simple - they just have to show up
  addToDisplay(this.innerHTML)
}

var basicOpClicked = function() {
  console.log(equation[currentIndex])
  // basic ops just need to check that they're coming after a number and not another operation
  if (!isNaN(equation[currentIndex]) || equation[currentIndex] == ")") {
    addToDisplay(this.innerHTML)
  }
}

// function that returns the nubmer of a given char in a certain array
function numContains(char) {
  // start a count
  count = 0
  // for each spot in array
  for (var i = 0; i < equation.length; i++) {
    // check if it's the char we care about
    if (equation[i] === char) {
      // increment the count
      count++
    }
  }
  // return the count
  return count
}

var parenClicked = function() {
  // if everything checks out
  if ((this.innerHTML === "(" && isNaN(equation[equation.length - 1])) || (this.innerHTML === ")" && numContains("(") > numContains(")")))  {
    addToDisplay(this.innerHTML)
    // if it would make too many close parentheses
  } else if (numContains("(") > numContains(")")){
    alert("Can't close a parentheses that wasn't opened!")
    // if the parentheses would come right after a number
  } else if (this.innerHTML === "(" && !isNaN(equation[equation.length - 1])) {
    addToDisplay("x")
    addToDisplay(this.innerHTML)
  }
}

function indexForward() {
  document.getElementById("extra-spaces").innerHTML = "$" + document.getElementById("extra-spaces").innerHTML
}

function indexBackwards() {
  document.getElementById("extra-spaces").innerHTML = document.getElementById("extra-spaces").innerHTML.slice(1)
}

// takes the long strings of numbers in 'equation' and make them one number
function condenseNum(arrayNum) {
  num = 0
  for (var i = 0; i < arrayNum.length; i++) {
    // take each number and multiply it by the proper power of 10
    num += arrayNum[arrayNum.length - i - 1] * Math.pow(10, i)
  }
  return num
}

// condenses all of the multi-digit number in 'equation'
function condenseEquation() {
  let i = 0
  let condensedEquation = []
  // while there are still chars left to figure out
  while (i < equation.length){
    // start out with a blank
    let longNum = []
    // while still working with a long number
    while (!isNaN(equation[i]) && i < equation.length) {
      // add digits to the number
      longNum.push(equation[i])
      i++
    }
    // if we were working with a number
    if (longNum.length != 0) {
      // condense it
      condensedEquation.push(condenseNum(longNum))
    }
    // if not at the end yet
    if (i < equation.length) {
      // add that operator
      condensedEquation.push(equation[i])
      i++
    }
  }
  return condensedEquation
}

// do the math to actually solve the equation
function parseEquation(eq) {
  // PEMDAS
  // look throuch whole equation
  for (var i = 0; i < eq.length; i++) {
    // if there's an open parentheses
    if (eq[i] === "(") {
      // starting the complicatedness! remember where the start is
      let start = i
      // assign an arbitrary end
      let end = start + 1
      // look through the equation from back to front
      for(var j = eq.length - 1; j > -1; j--) {
        // once we find the close parentheses
        if (eq[j] === ")") {
          // remeber when it ends
          end = j
          // no need to check further
          break
        }
      }
      // recursive call: figure out the answer to inside the parenthses and put it into the equation in place of the expression
      eq.splice(start, start - end, parseEquation(eq.slice(start + 1, end)))
      // no need to check further (still inside the is-there-an-open-parentheses if statement)
      break
    }
  }

  // E handling: look through array
  for (var i = 0; i < eq.length; i++) {
    // if ^ found
    if (eq[i] === "^") {
      // calculate the number before it to the power of the number after it
      miniAnswer = Math.pow(eq[i - 1],eq[i + 1])
      // put answer back in the mini expression's place
      eq.splice(i - 1, 3, miniAnswer)
    }
  }

  // M and D handling same idea
  for (var i = 0; i < eq.length; i++) {
    if (eq[i] === "x") {
      miniAnswer = eq[i - 1] * eq[i + 1]
      eq.splice(i - 1, 3, miniAnswer)
    }

    if (eq[i] === "/") {
      miniAnswer = eq[i - 1] / eq[i + 1]
      eq.splice(i - 1, 3, miniAnswer)
    }
  }

  // A and S handling
  for (var i = 0; i < eq.length; i++) {
    if (eq[i] === "+") {
      miniAnswer = eq[i - 1] + eq[i + 1]
      eq.splice(i - 1, 3, miniAnswer)
    }

    if (eq[i] === "-") {
      miniAnswer = eq[i - 1] - eq[i + 1]
      eq.splice(i - 1, 3, miniAnswer)
    }
  }
  
  // return the answer
  return eq[0]
}

function uncondenseEquation(answer) {
  // make the number a string
  answer = "" + answer
  stretched = []
  // stick each character of string in an array
  for (var i = 0; i < answer.length; i++) {
    stretched.push(answer[i])
  }
  return stretched
}

var equalsClicked = function() {
  // if the equation ends in an operator (that isn't a close parentheses)
  if (isNaN(equation.slice(-1)[0]) && equation.slice(-1)[0] != ")") {
    alert("Not valid expression")
    return
  }

  // if there are more open than close parentheses
  if (numContains("(") > numContains(")")) {
    // add the needed close parentheses
    for (var i = 0, n = numContains("(") - numContains(")"); i < n; i++) {
      equation.push(")")
    }
  }

  // condense the equation (put long numbers together)
  equation = condenseEquation(equation)
  // find and use the answer
  answer = parseEquation(equation)
  document.getElementById("display").innerHTML = answer
  // stretch the equation out
  equation = uncondenseEquation(answer)
  // reset the underline that shows the current index
  currentIndex = document.getElementById("display").innerHTML.length - 1
  document.getElementById("extra-spaces").innerHTML = ""
  for (var i = 0; i < document.getElementById("display").innerHTML.length; i++){
    indexForward()
  }
  console.log(currentIndex)
  console.log(equation)
}

var clearClicked = function() {
  // reset display and in-code equivalent
  document.getElementById("display").innerHTML = ""
  equation = [0]
  currentIndex = -1
  document.getElementById("extra-spaces").innerHTML = ""
}

var deleteClicked = function() {
  if (currentIndex == equation.length - 1 && currentIndex > 0) {
    document.getElementById("display").innerHTML = document.getElementById("display").innerHTML.substr(0, currentIndex)
    // delete from the in-code equivalent
    equation.pop()
    currentIndex -= 1
    indexBackwards()
  } else if (currentIndex != equation.length - 1 && currentIndex >= 0) {
    // for reference purposes
    old = document.getElementById("display").innerHTML
    // figure out and assign new string (with deleted char)
    document.getElementById("display").innerHTML = old.substr(0, currentIndex + 1) + old.substr(currentIndex + 2)
    equation.splice(currentIndex + 1, 1)
  }
}

var iMoverClicked = function() {
  // if it's a valid right click go right
  if (this.id === "index-right" && currentIndex < equation.length) {
    currentIndex++
    document.getElementById("index-display").innerHTML = " " + document.getElementById("index-display").innerHTML
  // same for left
  } else if (this.id === "index-left" && currentIndex > 0) {
    console.log(currentIndex)
    currentIndex -= 1
    indexBackwards()
  }
}
