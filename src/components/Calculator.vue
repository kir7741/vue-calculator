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
            <div class="lattice dark">
              <span>÷</span>
            </div>
            <div class="lattice dark">
              <span>×</span>
            </div>
            <div class="lattice dark">
              <span>+</span>
            </div>
            <div class="lattice dark">
              <span>−</span>
            </div>
          </div>
          <!-- 操作符區域結束 -->

        </div>

        <!-- 特殊功能按鈕開始 -->
        <div class="feature-pad">
          <div class="lattice">
            <span>AC</span>
          </div>
          <div class="lattice">
            <span>⌫</span>
          </div>
          <div class="lattice large">
            <div class="across">
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
import { constants } from 'crypto';
export default {
  name: 'Calculator',
  data() {
    return {
      numericCharacters: ['7', '8', '9', '4', '5', '6', '1', '2', '3', '0', '00', '.'],
      currentCalNum: '',
      calResult: '0',
      numWaitToCal: ''
    }
  },
  methods: {
    storeCalNum(num) {

      const isInit = this.calResult === '0';

      // 計算結果為初始值 0 ，且點擊的按鈕也是 0 的話，不新增數字
      if (
        isInit &&
        (
          num === '0' ||
          num === '00'
        )
      ) {
        return;
      }

      if (isInit) {
        this.calResult = num;
        this.currentCalNum = num;
        return;
      }

      this.calResult += num;
      this.currentCalNum += num;

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
