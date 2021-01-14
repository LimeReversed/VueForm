<template>
  <div class="formContainer">
    <div class="firstRow flexColumn">
      <label for="cardNumber">Card Number</label>
      <input v-model="card.number" type="text" id="cardNumber" placeholder="9999 9999 9999 9999">
    </div>
  
    <div class="secondRow flexColumn" >
      <label for="cardName">Card Name</label>
    <input v-model="card.name" type="text" id="cardName" placeholder="Jane Doe">
    </div>
    
    <div class="thirdRow flexBetween">
      <div style="width: 77%; text-align: left">
        <label>Expiration Date</label>
        <div class="dateContainer">
          <select v-model="card.month" id="month">
            <option v-for="monthNr in 12" :value="monthNr" :key="monthNr">{{monthNr}}</option>
          </select>
          <select v-model="card.year" id="year">
            <option v-for="yearNr in getYearArray()" :value="yearNr" :key="yearNr">{{yearNr}}</option>
          </select> 
        </div>
      </div>
      <div class="ccvContainer" style="width: 33%">
          <label for="ccvString">CCV</label>
          <input v-model="card.ccvString" type="text" id="ccvString" placeholder="999">
      </div>
    </div>
        
    <div class="fourthRow flexColumn">
      <button @click="validateForm">Submit</button>
    </div>
    
  </div>
  
</template>

<script>
export default {
  name: 'FormContainer',

  data() {
    return {
      card: {
        number: "",
        name: "",
        year: "",
        month: "",
        ccvString: ""
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

      if (year == "" && month == "" ){
        return false
      }

      let currentDate = new Date();
      let enteredDate = new Date(year, month);

      if (currentDate.setHours(0,0,0,0) <= enteredDate.setHours(0,0,0,0)){
        return true;
      }

      return false;
    },

    validateCCV(ccvString){

      if (ccvString == ""){
        return false
      }

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

      if (nrString == ""){
        return false
      }

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

      // Tar bort - och blanksteg från strängen
      let withoutDashes = nrString.replace("-", "");
      let withoutWhiteSpaces = withoutDashes.replace(/ /g, '');

      return withoutWhiteSpaces;
    },

    luhnsAlgorithm(nrString){

      let nrWithLastDigitRemoved = nrString.substring(0, nrString.length - 1);
      let sum = 0;

      for (let i = 0; i < nrWithLastDigitRemoved.length; i++){

        // Udda positioner ska multipliceras med 2
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
    
    // Om numret är 16 ska denna metod addera 1 och 6
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

/* Containers */
.formContainer{
  display: grid;
  grid-template-columns: 100%;
  grid-template-rows: auto auto auto auto;
  width: 30%;
  height: 40%;
  background-color: rgba(255, 255, 255, 0.9);
  border-radius: 10px;
  padding: 40px 50px;
  box-shadow: rgba(240, 240, 255, 0.3) 0px 0px 30px;
}

.dateContainer{
  display: flex;
  width: 100%;
}

.ccvContainer{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: flex-start;
}

/* Flex tools */
.flexColumn{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: flex-start;
}

.flexBetween{
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
}

/* Rows */
.firstRow{
  grid-row: 1;
}

.secondRow{
  grid-row: 2;
}

.thirdRow{
  grid-row: 3;
  
}

.fourthRow{
  grid-row: 4;
}

/* Tags */
label{
  font-size: small;
  margin-bottom: 3px;
}

input, select{
  height: 50px;
  width: 100%;
  border: solid 1px lightgray;
  border-radius: 5px;
  padding-left: 10px;
  font-size: medium;
  background-color: rgba(255, 255, 255, 0);
}

button{
  width: 100%;
  height: 50px;
  border-radius: 10px;
  font-size: medium;
  font-weight: bold;
  color: white;
  background-color: coral;
  box-shadow: rgba(255, 58, 23, 0.3) 5px 5px 10px;
  border: none;
}

select{
  width: 40%;
  margin-left: 10px;
}

select:first-child{
  margin: 0px;
}

</style>
