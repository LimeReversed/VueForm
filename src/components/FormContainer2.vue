<template>
  <div class="formContainer">
    <table>
      <tr>
        <td><label for="cardNumber">Card Number</label></td>
      </tr>
      <tr>
        <td><input v-model="card.number" type="text" id="cardNumber" placeholder="9999 9999 9999 9999"></td>
      </tr>
      <tr>
        <td>
          <label for="cardName">Card Name</label>
        </td>
      </tr>
      <tr>
        <td> <input v-model="card.name" type="text" id="cardName" placeholder="Jane Doe"></td>
      </tr>
      <tr>
        <td><label>Expiration Date</label></td>
        <td></td>
        <td><label for="ccvString">CCV</label></td>
      </tr>
      <tr>
        <td>
           <select v-model="card.month" id="month" style="width: 40%">
            <option v-for="monthNr in 12" :value="monthNr" :key="monthNr">{{monthNr}}</option>
          </select>
        </td>
        <td>
          <select v-model="card.year" id="year" style="width: 40%">
            <option v-for="yearNr in getYearArray()" :value="yearNr" :key="yearNr">{{yearNr}}</option>
          </select> 
        </td>
        <td>
          <input style="width: 80%" v-model="card.ccvString" type="text" id="ccvString" placeholder="999">
        </td>
      </tr>
    </table>
  </div>
  <!-- <div class="formContainer">

    <div class="thirdRow firstColumn flexColumn">
      <label>Expiration Date</label>
        <div class=flexRow style="width: 100%">
          <select v-model="card.month" id="month" style="width: 40%">
            <option v-for="monthNr in 12" :value="monthNr" :key="monthNr">{{monthNr}}</option>
          </select>
          <select v-model="card.year" id="year" style="width: 40%">
            <option v-for="yearNr in getYearArray()" :value="yearNr" :key="yearNr">{{yearNr}}</option>
          </select> 
        </div>
    </div>
    
    <div class="thirdRow secondColumn flexColumn" style="width: 100%; align-items: flex-end">
      <div style="width: 100%">
        <label for="ccvString">CCV</label>
        <input style="width: 80%" v-model="card.ccvString" type="text" id="ccvString" placeholder="999">
      </div>
    </div>
    
  </div> -->
  
</template>

<script>
export default {
  name: 'FormContainer',

  data() {
    return {
      card: {
        number: null,
        name: null,
        year: null,
        month: null,
        ccvString: null
      }
    }
  },

  methods: {
    getYearArray(){
      let currentYear = new Date().getFullYear();
      let yearsArray = [];

      for (let i = 0; i < 11; i++){
        let newYear = currentYear + i;
        yearsArray.push(newYear);
      }
      return yearsArray;
    },

    validateForm(){
      let numberIsValid = this.validateNumber(this.card.number);
      let dateIsValid = this.validateDate(this.card.month, this.card.year);
      let ccvIsValid = this.validateCCV(this.card.ccvString);

      if (numberIsValid === true && dateIsValid === true && ccvIsValid === true){
        alert("Success");
      }
      
      if (numberIsValid === false){
        alert("Number is not valid");
      }

      if (dateIsValid === false){
        alert("Date is not valid");
      }

      if (ccvIsValid === false){
        alert("CCV is not valid");
      }
    },
    
    validateDate(month, year){
      let currentDate = new Date();
      let enteredDate = new Date(year, month);

      if (currentDate.setHours(0,0,0,0) <= enteredDate.setHours(0,0,0,0)){
        return true;
      }

      return false;
    },

    validateCCV(ccvString){

      let correctLength = false;
      let allNumbers = false;
      ccvString = this.cleanNrString(ccvString);

      if (!isNaN(ccvString)){
        allNumbers = true;
      }

      if (ccvString.length === 3)
      {
        correctLength = true
      }

      return correctLength === true && allNumbers === true;
    },

    validateNumber(nrString){

      let allNumbers = false;
      let nrIsValid = false;

      let cleanedString = this.cleanNrString(nrString);

      if (!isNaN(cleanedString)){
        allNumbers = true;
        nrIsValid = this.luhnsAlgorithm(cleanedString);
      }

      return allNumbers === true && nrIsValid === true;
    },

    cleanNrString(nrString){
      let withoutDashes = nrString.replace("-", "");
      let withoutWhiteSpaces = withoutDashes.replace(/ /g, '');

      return withoutWhiteSpaces;
    },

    luhnsAlgorithm(nrString){

      let nrWithLastDigitRemoved = nrString.substring(0, nrString.length - 1);
      let sum = 0;

      for (let i = 0; i < nrWithLastDigitRemoved.length; i++){
        if (i % 2 === 0){
          let result = Number(nrWithLastDigitRemoved[i]) * 2;
          sum += this.addNumbersSeparately(result);
        }
        else{
          sum += Number(nrWithLastDigitRemoved[i])
        }
      }

      let sumAsString = sum.toString();
      let lastDigit = sumAsString[sumAsString.length - 1];
      let correctLastNr = 10 - Number(lastDigit);
      return Number(nrString[nrString.length - 1]) === correctLastNr;

    },

    addNumbersSeparately(nr){
    let nrString = nr.toString();
    let sum = 0;

    for (let i = 0; i < nrString.length; i++){
      sum += Number(nrString[i])
    }
    return sum;
  }
  
  },
}
</script>

<style scoped>

.formContainer{
  display: grid;
  grid-template-columns: auto auto;
  grid-template-rows: auto auto auto;
  width: 40%;
  height: 40%;
  background-color: white;
  border-radius: 10px;
  padding: 40px 50px;
  box-shadow: rgba(53, 53, 105, 0.363) 0px 0px 50px;
}

.flexRow{
  display: flex;
}

.flexColumn{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: flex-start;
}

.firstRow{
  grid-row: 1;
}

.secondRow{
  grid-row: 2;
}

.thirdRow{
  grid-row: 3;
}

.firstColumn{
  grid-column: 1;
}

.secondColumn{
  grid-column: 2;
}

/* .thirdColumn{
  grid-column: 3;
} */

/* .firstToThirdColumn{
  grid-column: 1 / span 3;
} */

.firstToSecondColumn{
  grid-column: 1 / span 2;
}

label{
  display: block;
}

input, select{
  height: 50px;
  width: 100%;
  border: solid 1px lightgray;
  border-radius: 5px;
}
</style>
