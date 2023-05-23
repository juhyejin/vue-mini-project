<template>
  <div class="main-contents">
    <AtomInput readonly :value="currentNum || '0'" />
    <div class="btn-group">
      <AtomButton
        v-for="btn in calculatorBtnList"
        :key="btn"
        @click-btn="clickBtn"
        class="btn"
      >
        {{ btn }}
      </AtomButton>
    </div>
  </div>
</template>

<script>
import AtomButton from "../components/Atom/button/AtomButton.vue";
import AtomInput from "@/components/Atom/input/AtomInput.vue";
export default {
  components: {
    AtomInput,
    AtomButton,
  },
  data() {
    return {
      calculatorBtnList:[
        "AC","+/-","%","/",
        "7","8","9","*",
        "4","5","6","-",
        "1","2","3","+",
        "0","00",".","="
      ],
      currentNum: "",
      prevNum: "",
      accNum: "",
      isOperator: false,
      isAccumulator: null,
    };
  },
  methods: {
    clickBtn: function (event) {
      const clickEl = event.target.innerText;
      if (isNaN(parseInt(clickEl))) {
        this.operatorEvent(clickEl);
      } else {
        this.numberEvent(clickEl);
      }
    },
    operatorEvent: function (operator) {
      const operatorEvent = () => {
        this.isOperator = true;
        this.prevNum = this.currentNum;
      }
      switch (operator){
        case "AC":
          this.currentNum = "0";
          this.isOperator = false;
          this.isAccumulator = false;
          break;
        case "+/-":
          if (this.currentNum >= 0) this.currentNum = this.currentNum * -1;
          else this.currentNum = Math.abs(this.currentNum);
          break;
        case "%":
          this.currentNum = parseFloat(this.currentNum) / 100;
          this.isOperator = true;
          break;
        case "=":
          if (!this.isOperator) {
            this.currentNum = this.isAccumulator(
              Number(this.prevNum),
              Number(this.accNum)
            );
            this.prevNum = this.currentNum;
          } else {
            this.currentNum = this.isAccumulator(
              Number(this.prevNum),
              Number(this.currentNum)
            );
            this.isOperator = true;
          }
          break;
        case ".":
          // eslint-disable-next-line no-case-declarations
          const includesDot = String(this.currentNum).includes(".");
          if (!includesDot) this.currentNum = this.currentNum + ".";
          break;
        case "+":
          operatorEvent();
          this.isAccumulator = (a, b) => {
            return a + b;
          };
          break;
        case "-":
          operatorEvent();
          this.isAccumulator = (a, b) => {
            return a - b;
          };
          break;
        case "*":
          operatorEvent();
          this.isAccumulator = (a, b) => {
            return a * b;
          };
          break;
        case "/":
          operatorEvent();
          this.isAccumulator = (a, b) => {
            return a / b;
          };
          break;
      }
    },
    numberEvent: function (num) {
      if (this.isOperator) {
        this.currentNum = "";
        this.isOperator = false;
        this.accNum = "";
      }
      if (this.currentNum === "0") {
        if (num === "0" || num === "00") {
          this.currentNum = "0";
        } else {
          this.currentNum = num;
          this.accNum = num;
        }
      } else {
        this.currentNum += num;
        this.accNum = num;
      }
    },
  },
};
</script>

<style>
.main-contents {
  max-width: 50%;
  margin: 0 auto;
  text-align: center;
}
.btn-group{
  display: grid;
  grid-template-columns: repeat(4, 50px);
  gap: 10px;
  justify-content: center;
  align-self: center;
}
.btn-group .btn {
  width: 50px;
  height: 50px;
  background: #dcfaff;
  font-size: 18px;
  color: #3c7f68;
  outline: none;
  border: none;
  border-radius: 10px;
  box-shadow: 0 5px 2px #3c7f68;
  cursor: pointer;
}
.btn-group .btn:nth-child(-n + 3){
  background: #ffffff;
}
.btn-group .btn:nth-child(4n){
  background: #78ccc5;
}
.btn-group .btn:hover {
  background: #3c7f68;
  color: #dcfaff;
  transform: translateY(5px);
  box-shadow: none;
}

</style>
