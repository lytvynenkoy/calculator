<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <div class="col-md-4 m-3">
      <table class="table table-bordered">

        <tbody>
        <tr class="output">
          <td colspan="4">
            {{ output || 0 }}
          </td>
        </tr>
        <tr>
          <td v-on:click="clearField">C</td>
          <td @click="setState">+/-</td>
          <td @click="setPercent">%</td>
          <td class="lastColumn" @click="processOutput('divide')">/</td>
        </tr>
        <tr>
          <td @click="getNumber('7')">7</td>
          <td @click="getNumber('8')">8</td>
          <td @click="getNumber('9')">9</td>
          <td class="lastColumn" @click="processOutput('multiply')">x</td>
        </tr>
        <tr>
          <td @click="getNumber('4')">4</td>
          <td @click="getNumber('5')">5</td>
          <td @click="getNumber('6')">6</td>
          <td class="lastColumn" @click="processOutput('minus')">-</td>
        </tr>
        <tr>
          <td @click="getNumber('1')">1</td>
          <td @click="getNumber('2')">2</td>
          <td @click="getNumber('3')">3</td>
          <td class="lastColumn" @click="processOutput('add')">+</td>
        </tr>
        <tr>
          <td colspan="2"  v-on:click="getNumber('0')">0</td>
          <td @click="getDot()">.</td>
          <td class="lastColumn" @click="updateOutput">=</td>
        </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    msg: String
  },
  data(){
    return {
      output:'',
      previousValue: null,
      oprationFired: false,
    }
  },
  methods: {
    clearField() {
      this.output = '';
    },
     setState() {
      this.output = this.output[0] === '-' ? this.output.slice(1) : `-${this.output}`
     },
    setPercent() {
      this.output = parseFloat(this.output) / 100;
    },
    getNumber(number) {

      if (this.oprationFired){
        this.output = '';
        this.oprationFired = false;
      }

      this.output = `${this.output}${number}`;
    },
    getDot() {
      if(this.output.indexOf('.') === -1) {
        this.output = this.output + '.'
      }
    },
    processOutput(string) {

      if(string =='add'){
        this.operation = (a, b) => {
          return parseFloat(a) + parseFloat(b);
        }
      } else if(string =='minus'){
        this.operation = (a, b) => {
          return parseFloat(a) - parseFloat(b);
        }
      } else if(string =='multiply'){
        this.operation = (a, b) => {
          return parseFloat(a) * parseFloat(b);
        }
      } else if(string =='divide'){
        this.operation = (a, b) => {
          return parseFloat(a) / parseFloat(b);
        }
      }


      this.previousValue = this.output;
      this.oprationFired = true;
    },
    updateOutput() {
      this.output = `${this.operation(this.previousValue, this.output)}`;
      this.previousValue = null;
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

.output {
  color: #fff;
  background-color: #000;
}

.lastColumn {
  color: #fff;
  background-color: orange;
}

.lastColumn:active {
  background-color: #000;
}
</style>
