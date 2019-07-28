<template>
  <div class="wrapper">
    <div class="cal-box">
      <div class="cal-header">

      </div>
      <div class="cal-body">
        <div class="number-pad">

          <!-- 數字按鈕區域開始 -->
          <div>
            <div
              class="lattice"
              v-for="(char, index) in numericCharacters"
              :key="index"
            >
              <span >
                {{ char }}
              </span>
            </div>
          </div>
          <!-- 數字按鈕區域結束 -->

          <!-- 操作符區域開始 -->
          <div>
            <div class="lattice">
              <span>÷</span>
            </div>
            <div class="lattice">
              <span>×</span>
            </div>
            <div class="lattice">
              <span>+</span>
            </div>
            <div class="lattice">
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
export default {
  name: 'Calculator',
  data() {
    return {
      numericCharacters: ['.', '00', '0', '1', '2', '3', '4', '5', '6', '7', '8', '9']
    }
  }
}
</script>

<style scoped lang="scss">
@import '../scss/_mixin.scss';
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
  height: 110px;
  border-top-left-radius: 10px;
  border-top-right-radius: 10px;
  background-color: $darkBlue;
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
    &.large {
      flex: 2;
    }
  }
}
.across {
  display: flex;
  width: 100%;
}
.lattice {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 80px;
  color: #ffffff;
  span {
    display: inline-flex;  
    align-items: center;
    justify-content: center;  
    padding: 8px;  
    width: 100%;
    height: 100%;
    font-size: 24px;
  }
}
</style>
