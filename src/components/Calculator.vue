<script setup>
import {reactive} from "vue";

const payload = reactive({
  output:'',
  operation:'',
  previousValue: null,
  operationFired:false
})

const clearField = () => {
  payload.output = ''
}
const setNegativeOrPositive =() =>{
  payload.output = payload.output[0] === '-' ? payload.output.slice(1) : `-${payload.output}`
}
const calculatePercentage =() =>{
  payload.output = parseFloat(payload.output)/100
}
const getNumber = (number) => {
  if (payload.operationFired) {
    payload.output = ''
    payload.operationFired = false
  }
  payload.output = `${payload.output}${number}`
}
const getDot = () => {
  if(payload.output.indexOf('.') === -1){
    payload.output = payload.output+'.'
  }
}
const processOutput = (string) => {
  if(string === 'add') {
    payload.operation = (a, b) => {
      return parseFloat(a)+parseFloat(b)
    }
  }else if (string === 'subtract') {
    payload.operation = (a, b) => {
      return parseFloat(a)-parseFloat(b)
    }
  }else if (string === 'multiply') {
    payload.operation = (a, b) => {
      return parseFloat(a) * parseFloat(b)
    }
  }else if (string === 'divide') {
    payload.operation = (a, b) => {
      return parseFloat(a) / parseFloat(b)
    }
  }
  payload.previousValue = payload.output
  payload.operationFired = true
}

const updateOutput = () => {
  payload.output = `${payload.operation(payload.previousValue, payload.output)}`
  payload.previousValue = null
}

</script>

<template>
  <div class="hello">
  <h1> MacOS Calculator Clone </h1>
   <div class="col-md-4 m-3">
     <table class="table table-bordered">
       <tbody>
       <tr class="output">
          <td colspan="4">
            {{ payload.output || 0 }}
          </td>
       </tr>
       <tr>
         <td class="firstRow" @click="clearField">C</td>
         <td class="firstRow" @click="setNegativeOrPositive">+/-</td>
         <td class="firstRow" @click="calculatePercentage">%</td>
         <td @click="processOutput('divide')" class="lastColumn"><i class ="fas fa-divide"></i></td>
       </tr>
       <tr>
         <td @click="getNumber('7')" class="number">7</td>
         <td @click="getNumber('8')" class="number">8</td>
         <td @click="getNumber('9')" class="number">9</td>
         <td @click="processOutput('multiply')" class="lastColumn">x</td>
       </tr>
       <tr>
         <td @click="getNumber('4')" class="number">4</td>
         <td @click="getNumber('5')" class="number">5</td>
         <td @click="getNumber('6')" class="number">6</td>
         <td @click="processOutput('subtract')" class="lastColumn">-</td>
       </tr>
       <tr>
         <td @click="getNumber('1')" class="number">1</td>
         <td @click="getNumber('2')" class="number">2</td>
         <td @click="getNumber('3')" class="number">3</td>
         <td @click="processOutput('add')" class="lastColumn">+</td>
       </tr>
       <tr>
         <td @click="getNumber('0')" class="number" colspan="2">0</td>
         <td @click="getDot" class="number">.</td>
         <td @click="updateOutput" class="lastColumn">=</td>
       </tr>
       </tbody>
     </table>
   </div>

  </div>
</template>

<style scoped>
.hello {
  height: 100vh;
  width: 100vw;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
h1 {

}
tr {
  text-align: center;
}
td {
  user-select: none;
}
.firstRow {
  background-color: #c0c0c0;
}
.firstRow:active {
  background-color: #e6e6e6;
}
.number {
  background-color: #d0d0d0;
  color: #000000;
}
.number:active {
  background-color: #f6f6f6;
}
.output {
  background-color: #32322f;
  color: #ffffff;
}
.lastColumn {
  background-color: #ffa500;
  color: #ffffff;
  cursor: pointer;
}
.lastColumn:active {
  background-color: #333333;
  color: #ffffff;
}
</style>