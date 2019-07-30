<template>
  <div class="wrapper">
    <div class="cal-box">
      <div class="cal-header">
        <div>
          {{ currentCalNum }}
        </div>
        <div>
          {{ calResult }}
        </div>
      </div>
      <div class="cal-body">
        <div class="number-pad">

          <!-- 數字按鈕區域開始 -->
          <div>
            <div
              class="lattice"
              v-for="(char, index) in numericCharacters"
              :key="index"
              @click="storeCalNum(char)"
            >
              <span >
                {{ char }}
              </span>
            </div>
          </div>
          <!-- 數字按鈕區域結束 -->

          <!-- 操作符區域開始 -->
          <div>
            <div 
              class="lattice dark"
              @click="addOperator(operatorType.DIVIDE)"
            >
              <span>÷</span>
            </div>
            <div 
              class="lattice dark"
              @click="addOperator(operatorType.MULTIPLY)"
            >
              <span>×</span>
            </div>
            <div 
              class="lattice dark"
              @click="addOperator(operatorType.PLUS)"
            >
              <span>+</span>
            </div>
            <div 
              class="lattice dark"
              @click="addOperator(operatorType.MINUS)"
            >
              <span>−</span>
            </div>
          </div>
          <!-- 操作符區域結束 -->

        </div>

        <!-- 特殊功能按鈕開始 -->
        <div class="feature-pad">
          <div 
            class="lattice"
            @click="reset()"
          >
            <span>AC</span>
          </div>
          <div 
            class="lattice"
            @click="pop()"
          >
            <span>⌫</span>
          </div>
          <div class="lattice large">
            <div 
              class="across"
              @click="doCalResult()"
            >
              <span></span>
              <span>=</span>
            </div>
          </div>
        </div>
        <!-- 特殊功能按鈕結束 -->

      </div>
    </div>
  </div>
</template>

<script>

// Enums
import operatorType from '../enum/operator-type.js';

export default {
  name: 'Calculator',
  data() {
    return {
      operatorType: operatorType,
      numericCharacters: ['7', '8', '9', '4', '5', '6', '1', '2', '3', '0', '00', '.'],
      currentCalNum: '',
      calResult: '0',
      lastOperator: '',
      lastNumber: '',
      isSelectOperator: false,
      canPop: true
    }
  },
  methods: {
    storeCalNum(num) {

      const isInit = this.calResult === '0';
      const clickZero = num === '0' || num === '00';

      // 計算結果為初始值 0 ，且點擊的按鈕也是 0 的話，不新增數字
      if (isInit && clickZero) {
        return;
      }

      if (isInit || this.isSelectOperator) {

        if (clickZero) {
          this.calResult = '0'
        } else {
          this.calResult = num;
        }
        
      } else {
        this.calResult += num;
      }

      // 暫存最後一組數字
      this.lastNumber = this.calResult;

      this.canPop = true;
      this.isSelectOperator = false;

    },
    addOperator(operator) {

      // 同樣的操作，且在選擇運算符的過程不處理
      if (
        operator === this.lastOperator &&
        this.isSelectOperator
      ) {
        return;
      }

      if (this.isSelectOperator) {

        // 如果是在選擇運算符的過程，刪除之前的運算符
        this.currentCalNum = this.currentCalNum.slice(0, -1);

      } else {
        this.currentCalNum += this.calResult;
      }

      this.currentCalNum += this.getOperator(operator);

      // 暫存前一次操作符號
      this.lastOperator = operator;

      this.canPop = false;
      this.isSelectOperator = true;

    },
    getOperator(type) {

      switch (type) {

        case operatorType.PLUS:
          return '+';

        case operatorType.MINUS:
          return '-';

        case operatorType.MULTIPLY:
          return 'x';

        case operatorType.DIVIDE:
          return '÷';

        default:
          return '';

      }
    },
    replaceOperator(calData) {

      calData = calData.replace(/x/g, '*');
      calData = calData.replace(/÷/g, '/');

      return calData;
      
    },
    doCalResult() {

      // 沒有運算符則不處理
      if (!this.lastOperator) {
        return;
      }

      let calData = '';

      if (!this.currentCalNum) {

        const operator = this.getOperator(this.lastOperator);

        calData = this.calResult + operator + this.lastNumber;
        calData = this.replaceOperator(calData);
        
      } else {
        calData = this.replaceOperator(this.currentCalNum + this.calResult);
      }

      const result = eval(calData);
  
      this.calResult = String(result);
      this.isSelectOperator = false;
      this.canPop = false;
      this.currentCalNum = '';

    },
    pop() {

      if (this.canPop) {

        if (this.calResult.length <= 1) {
          this.calResult = '0';
          return;
        }

        this.calResult = this.calResult.slice(0, -1);

      }

    },
    reset() {
      this.currentCalNum = '';
      this.calResult = '0';
      this.lastOperator = '';
      this.lastNumber = '';
      this.isSelectOperator = false;
    }
  }
}
</script>

<style scoped lang="scss">
@import '../scss/_mixin.scss';
@mixin hiddenText {
  overflow: hidden;
  text-overflow: ellipsis;
}
.wrapper {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100%;
}
.cal-box {
  width: 350px;
  height: 525px;
}
.cal-header {
  display: flex;
  flex-direction: column;
  border-top-left-radius: 10px;
  border-top-right-radius: 10px;
  padding: 10px;
  background-color: $darkBlue;
  height: 110px;
  div:first-child {
    @include hiddenText;
    flex: 1;
    color: #ffffff;
    text-align: right;
  }
  div:last-child {
    @include hiddenText;
    flex: 2;
    color: #ffffff;
    text-align: right;
    line-height: 60px;
    font-weight: bold;
  }
}
.cal-body {
  display: flex;
  flex-direction: column;
  height: 415px;
  background-color: $primaryBlue;
  border-bottom-left-radius: 10px;
  border-bottom-right-radius: 10px;
  .number-pad {
    display: flex;
    flex-direction: row;
    div:first-child {
      display: flex;
      flex-wrap: wrap;
      flex: 3;
      .lattice {
        flex: 0 0 33.33333%;
      }
    }
    div:last-child {
      flex: 1;
    }
  }
}
.feature-pad {
  display: flex;
  .lattice {
    flex: 1;
    span {
      color: $lightBlueText;
    }
    &.large {
      flex: 2;
    }
  }
}
.across {
  display: flex;
  width: 100%;
  height: 100%;
  background-image: linear-gradient(to right, $lightBlueText , $lightPurpleText);
  border-radius: 10px;
}
.lattice {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 10px;  
  height: 80px;
  color: #ffffff;
  cursor: pointer;
  span {
    display: inline-flex;  
    align-items: center;
    justify-content: center; 
    border-radius: 10px; 
    width: 100%;
    height: 100%;
    font-size: 24px;
  }
  &.dark span {
    background-color: $darkBlue;
  }
}
</style>
