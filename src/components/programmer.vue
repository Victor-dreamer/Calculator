<template>
  <div class='programmer'>
    <div class="programmer-system">
      <div :class="[(systemType === 16)?'programmer-system-item current-system':'programmer-system-item']" @click="changeSystem(16, showHEX)">
        <div class="system-name">HEX</div>
        <div class="system-val">{{ showHEX }}</div>
      </div>
      <div :class="[(systemType === 10)?'programmer-system-item current-system':'programmer-system-item']" @click="changeSystem(10)">
        <div class="system-name">DEC</div>
        <div class="system-val">{{ showDEC }}</div>
      </div>
      <div :class="[(systemType === 8)?'programmer-system-item current-system':'programmer-system-item']" @click="changeSystem(8, showOCT)">
        <div class="system-name">OCT</div>
        <div class="system-val">{{ showOCT }}</div>
      </div>
      <div :class="[(systemType === 2)?'programmer-system-item current-system':'programmer-system-item']" @click="changeSystem(2, showBIN)">
        <div class="system-name">BIN</div>
        <div class="system-val">{{ showBIN }}</div>
      </div>
    </div>
    <div class="programmer-menu">
      <div @click="()=>{this.menu=1}" :class="[(menu===1)?'iconfont current':'iconfont']">&#xe708;</div>
      <div @click="()=>{this.menu=0}" :class="[(menu===0)?'iconfont current':'iconfont']">&#xe954;</div>
      <div class="chooseSystem" @click="changeAccuracy">{{ showAccuracy }}</div>
      <span class="memory-button"><button @click="changeShowMemory" :disabled="!this.memoryList.length">M</button></span>
      <span class="memory-button" @click="memorySave()"><button>MS</button></span>
    </div>
    <div v-if="menu" class="programmer-content">
      <div class="programmer-button">
        <!-- 1 -->
        <div class="programmer-button-item"><button class="background" @click="inputOperator('Lsh')">Lsh</button></div>
        <div class="programmer-button-item"><button class="background" @click="inputOperator('Rsh')">Rsh</button></div>
        <div class="programmer-button-item"><button class="background" @click="inputOperator('|')">Or</button></div>
        <div class="programmer-button-item"><button class="background" @click="inputOperator('^')">Xor</button></div>
        <div class="programmer-button-item"><button class="background" @click="inputunaryoperator('~')">Not</button></div>
        <div class="programmer-button-item"><button class="background" @click="inputOperator('&')">And</button></div>
        <!-- 2 -->
        <div class="programmer-button-item"><button class="background">↑</button></div>
        <div class="programmer-button-item"><button class="background" @click="inputOperator('%')">Mod</button></div>
        <div class="programmer-button-item"><button class="background" @click="clearInput">CE</button></div>
        <div class="programmer-button-item"><button class="background" @click="clearAll">C</button></div>
        <div class="programmer-button-item"><button class="background" @click="clearOne">←</button></div>
        <div class="programmer-button-item">
          <button class="background operator" @click="inputOperator('÷')">÷</button>
        </div>
        <!-- 3 -->
        <div class="programmer-button-item">
          <button :disabled='systemType!==16' @click="inputNum('A')">A</button>
        </div>
        <div class="programmer-button-item">
          <button :disabled='systemType!==16' @click="inputNum('B')">B</button>
        </div>
        <div class="programmer-button-item">
          <button @click="inputNum(7)" :disabled='systemType===2'>7</button>
          </div>
        <div class="programmer-button-item">
          <button @click="inputNum(8)" :disabled="systemType===2||systemType===8">8</button>
        </div>
        <div class="programmer-button-item">
          <button @click="inputNum(9)" :disabled="systemType===2||systemType===8">9</button>
        </div>
        <div class="programmer-button-item">
          <button class="background operator" @click="inputOperator('×')">×</button>
        </div>
        <!-- 4 -->
        <div class="programmer-button-item">
          <button :disabled='systemType!==16' @click="inputNum('C')">C</button>
        </div>
        <div class="programmer-button-item">
          <button :disabled='systemType!==16' @click="inputNum('D')">D</button>
        </div>
        <div class="programmer-button-item">
          <button @click="inputNum(4)" :disabled='systemType===2'>4</button>
        </div>
        <div class="programmer-button-item">
          <button @click="inputNum(5)" :disabled='systemType===2'>5</button>
        </div>
        <div class="programmer-button-item">
          <button @click="inputNum(6)" :disabled='systemType===2'>6</button>
        </div>
        <div class="programmer-button-item">
          <button class="background operator" @click="inputOperator('-')">-</button>
        </div>
        <!-- 5 -->
        <div class="programmer-button-item">
          <button :disabled='systemType!==16' @click="inputNum('E')">E</button>
        </div>
        <div class="programmer-button-item">
          <button :disabled='systemType!==16' @click="inputNum('F')">F</button>
        </div>
        <div class="programmer-button-item">
          <button @click="inputNum(1)">1</button>
        </div>
        <div class="programmer-button-item">
          <button @click="inputNum(2)" :disabled='systemType===2'>2</button>
        </div>
        <div class="programmer-button-item">
          <button @click="inputNum(3)" :disabled='systemType===2'>3</button>
        </div>
        <div class="programmer-button-item">
          <button class="background operator" @click="inputOperator('+')">+</button>
        </div>
        <!-- 6 -->
        <div class="programmer-button-item"><button class="background" @click="inputBrackets('(')">(</button></div>
        <div class="programmer-button-item"><button class="background" @click="inputBrackets(')')">)</button></div>
        <div class="programmer-button-item"><button class="background" @click="inputunaryoperator('neg')">±</button></div>
        <div class="programmer-button-item"><button @click="inputNum(0)">0</button></div>
        <div class="programmer-button-item background">.</div>
        <div class="programmer-button-item">
          <button class="background operator" @click="getResult">=</button>
        </div>
      </div>
    </div>
    <div v-else>
      <bitKeyboard :accuracy="accuracy" :systemType="systemType"></bitKeyboard>
    </div>
  </div>
</template>

<script>
import { mapState } from 'vuex'
import bitKeyboard from '../components/bitKeyboard.vue'

export default {
  name: 'programmer',
  components: {
    bitKeyboard
  },
  data: function () {
    return {
      menu: 1,
      isbitkeyboard: false,
      bitkeyboardType: 64,
      // 存放输入的数字信息
      inputScren: [],
      // 判断是否可以输入操作符
      isInputOperator: false,
      // 存放计算的表达式数组
      expressions: [],
      // 存储上一次运算的最后一步
      history: {},
      accuracy: 0,
      accuracyCenter: 0,
      // 记录左括号数量
      leftBrackets: 0
    }
  },
  watch: {
    // 提交当前显示的数值
    currentShow: function (val) {
      this.$store.commit('commitNum', val)
    },
    // 提交表达式
    expressions: function (val) {
      this.$store.commit('commitExpression', val)
    }
  },
  computed: {
    ...mapState(['currentNum', 'memoryList', 'showMemory', 'systemType']),
    accuracyType: function () {
      switch (this.accuracy) {
        case 32:
          return 32
        case 48:
          return 16
        case 56:
          return 8
        case 0:
          return 64
        default:
          return 64
      }
    },
    // 当前屏幕显示的数值
    currentShow: function () {
      return this.currentNum || this.inputScren.join('') || 0
    },
    // 不同进制显示的数值
    showDEC: function () {
      // 判断当前输入的数值是否超过当前精度所能表示的最大正数
      // 若超过，表示为负数
      if (this.currentShow > Math.pow(2, this.accuracyType - 1)) {
        return this.getDEC(parseInt(this.currentShow, this.systemType).toString(2).split(''))
      } else {
        return parseInt(this.currentShow, this.systemType)
      }
    },
    showHEX: function () {
      return parseInt(this.showBIN, 2).toString(16)
    },
    showOCT: function () {
      return parseInt(this.showBIN, 2).toString(8)
    },
    showBIN: function () {
      if (this.showDEC < 0) {
        return this.getComplement(this.showDEC).join('')
      } else {
        return this.showDEC.toString(2)
      }
    },
    showAccuracy: function () {
      switch (this.accuracyType) {
        case 64:
          return 'QWORD'
        case 32:
          return 'DWORD'
        case 16:
          return 'WORD'
        case 8:
          return 'BYTE'
        default:
          return 'QWORD'
      }
    }
  },
  methods: {
    //  检查数字是否符合当前精度
    isInAccuracy (num) {
      let temp = num.toString(2)
      // console.log('结果精度' + temp.length)
      // console.log(temp)
      if (temp.length > this.accuracyType) {
        return false
      } else {
        return true
      }
    },

    changeShowMemory () {
      this.$store.commit('changeShowMemory', !this.showMemory)
    },
    // 判断是否操作符
    isOperator (sign) {
      // eslint-disable-next-line no-useless-escape
      const reg = /[\+, \*, \÷, \(, \-, \%, \|, \^, \&]/
      return reg.test(sign)
    },
    // 改变精度
    changeAccuracy () {
      this.accuracyCenter++
      if (this.accuracyCenter > 3) {
        this.accuracyCenter = 0
      }
      switch (this.accuracyCenter) {
        case 0:
          this.accuracy = 0
          break
        case 1:
          this.accuracy = 32
          break
        case 2:
          this.accuracy = 48
          break
        case 3:
          this.accuracy = 56
          break
        default:
          this.accuracy = 0
          break
      }
    },
    changeSystem (val) {
      let num
      switch (val) {
        case 16:
          num = this.showHEX
          break
        case 10:
          num = this.showDEC
          break
        case 8:
          num = this.showOCT
          break
        case 2:
          num = this.showBIN
          break
        default:
          break
      }
      this.$store.commit('commitCurrentNum', num)
      this.inputScren = []
      this.$store.commit('changeSystemType', val)
    },
    // 输入及计算操作
    // 输入括号
    inputBrackets (val) {
      // 表达式为空时，左括号直接加入
      if (this.expressions.length === 0) {
        if (val === '(') {
          // 略过当前输入的数值
          if (this.inputScren.length !== 0) {
            this.$store.commit('commitCurrentNum', this.currentShow)
            this.inputScren = []
          }
          this.expressions.push(val)
          this.leftBrackets++
        }
      } else {
        if (val === '(') {
          if (!isNaN(this.expressions[this.expressions.length - 1])) {
            return
          }
          if (this.expressions[this.expressions.length - 1] === ')') {
            return
          }
          this.expressions.push(val)
          this.leftBrackets++
        }
        if (val === ')') {
          if (this.inputScren.length !== 0) {
            this.expressions.push(this.currentShow)
            this.expressions.push(val)
            this.inputScren = []
            this.isInputOperator = false
            return
          }
          if (this.isInputOperator === true) {
            return
          }
          if (this.expressions[this.expressions.length - 1] === '(') {
            return
          }
          if (this.leftBrackets <= 0) {
            return
          }
          this.expressions.push(val)
          this.leftBrackets--
        }
      }
    },
    // 输入数字
    inputNum (value) {
      if (this.currentNum !== '') {
        this.$store.commit('commitCurrentNum', '')
      }
      if (this.inputScren.length === 0) {
        this.inputScren.push(value)
      } else {
        if (this.inputScren[0] === 0) {
          this.inputScren[0] = value
        } else {
          let temp = this.inputScren.join('') + '' + value
          if (this.isInAccuracy(parseInt(temp, this.systemType))) {
            this.inputScren.push(value)
          }
        }
      }
      this.isInputOperator = false
    },
    // 输入运算符
    inputOperator (value) {
      // 判断是否有上一步运算结果
      if (this.currentNum !== '') {
        this.expressions.push(this.currentNum)
        this.$store.commit('commitCurrentNum', '')
        this.expressions.push(value)
        this.isInputOperator = true
        return
      }
      // 如果当前有输入的数字，先将数字添加进表达式
      if (this.inputScren.length !== 0) {
        this.expressions.push(this.currentShow)
        this.inputScren = []
        this.expressions.push(value)
        this.isInputOperator = true
        return
      }
      if (this.inputScren.length === 0) {
        // 判断是表达式末尾是否为操作符，若是则进行替换
        if (this.isInputOperator) {
          this.expressions[this.expressions.length - 1] = value
        }
      }
    },
    // 提交'='号，即获取结果
    getResult () {
      if (this.inputScren.length === 0 && this.expressions.length === 0) {
        // 将当前结果进行上一个表达式的最后一步操作运算，待完善
        if (JSON.stringify(this.history) !== '{}') {
          if (this.currentNum === '') {
            this.expressions.push(0)
          }
          if (this.currentNum !== '') {
            this.expressions.push(this.currentNum)
          }
          this.expressions.push(this.history['op'])
          this.expressions.push(this.history['num'])
          this.$store.commit('commitCurrentNum', '')
        }
      }
      if (this.inputScren.length !== 0) {
        this.expressions.push(this.currentShow)
        this.inputScren = []
      }
      if (this.isOperator(this.expressions[this.expressions.length - 1])) {
        this.expressions.pop()
      }
      // 对不同进制进行转换
      let reg16 = /^([0-9|a-f|A-F]*)$/
      let exp = this.expressions.join(' ').replace(new RegExp('×', 'g'), '*').replace(new RegExp('÷', 'g'), '/').split(' ')
      let arr = exp.map((item) => {
        if (item === '|') {
          return item
        } else if (!isNaN(item) || reg16.test(item)) {
          // 将参与运算的数字转成10进制的
          return parseInt(item, this.systemType)
        } else {
          return item
        }
      })
      console.log('表达式：' + exp)
      console.log('表达式：' + arr)
      let res = this.computeExpression(this.changeExpression(arr))
      // 对结果进行精度判断
      res = this.accuracyLimmit(res)
      // 判断是否转换负数
      if (this.systemType === 10) {
        res = this.getDEC(res.toString(2).split(''))
      } else {
        res = res.toString(this.systemType)
      }
      this.$store.commit('commitCurrentNum', res)
      this.expressions = []
      this.isInputOperator = false
    },
    inputunaryoperator (value) {
      let res
      if (this.currentNum !== '') {
        res = this.computeUnaryoperator(value, this.currentNum)
        this.$store.commit('commitCurrentNum', res)
        return
      }
      if (this.inputScren.length !== 0) {
        res = this.computeUnaryoperator(value, this.currentShow)
        this.inputScren = []
        this.$store.commit('commitCurrentNum', res)
      }
    },
    // 转换数值的正负
    getNegative (value) {
      return ~value + 1
    },
    // 判断是否溢出当前精度，若溢出，则进行截取
    accuracyLimmit (num) {
      let temp = num.toString(2)
      if (temp.length > this.accuracyType) {
        // 截取超出精度部分
        temp = temp.split('')
        temp = temp.splice(temp.length - this.accuracyType)
        temp = parseInt(temp.join(''), 2)
        return temp
      } else {
        return num
      }
    },
    // 清除操作
    clearOne () {
      if (this.inputScren.length !== 0) {
        this.inputScren.pop()
      }
    },
    clearInput () {
      this.inputScren = []
    },
    clearAll () {
      this.inputScren = []
      this.expressions = []
      this.$store.commit('commitCurrentNum', '')
      this.history = {}
    },
    // 内存操作
    memorySave () {
      let value = parseInt(this.currentShow, this.systemType)
      this.$store.commit('memorySave', value)
    },
    // 将负数十进制转为2进制补码
    getComplement (num) {
      let temp = Math.abs(num).toString(2)
      let arr = temp.split('').slice(1)
      console.log('转换数组：' + arr)
      if (temp.length < this.accuracyType) {
        for (let i = 0; i < this.accuracyType - temp.length; i++) {
          arr.splice(0, 0, 0)
        }
        console.log('补充精度：' + arr)
      }
      // 取反及 +1
      temp = this.getAddOne(this.getNegArr(arr))
      console.log('补码数组：' + [1].concat(temp))
      return [1].concat(temp)
    },
    // 获取负数的10进制表示
    getDEC (arr) {
      if ((arr[0] === 1 || arr[0] === '1') && arr.length === this.accuracyType) {
        let temp = (parseInt(this.getNegArr(arr.slice(1)).join(''), 2) + 1)
        // console.log('转换了' + temp)
        return temp * -1
      } else {
        return parseInt(arr.join(''), 2)
      }
    },
    // 二进制加1
    getAddOne (arr) {
      let i = arr.length - 1
      while (i >= 0) {
        if (arr[i] + 1 >= 2) {
          arr[i] = 0
          i--
        } else {
          arr[i] = 1
          break
        }
      }
      // console.log('加一：' + arr)
      return arr
    },
    // 对一个二进制数组进行按位取反
    getNegArr (arr) {
      const temp = arr.map((val) => {
        if (val + 1 >= 2) {
          return 0
        } else {
          return 1
        }
      })
      // console.log('取反：' + temp)
      return temp
    },
    // 将中缀表达式转换为后缀表达式
    changeExpression (expression) {
      let stack = []
      // let expression = [12, '+', 20, '*', '(', 2, '-', 10, ')', '+', 50, '/', 10]
      let temp = []
      // let reg = /^([0-9|a-z|A-Z]*)$/
      // const op = /[\+, \*, \÷, \(, \), \-, \~, \%, \|, \^, \&]/
      let map = {
        '(': 1,
        '|': 2,
        '^': 3,
        '&': 4,
        'Lsh': 5,
        'Rsh': 5,
        '+': 6,
        '-': 6,
        '*': 7,
        '/': 7,
        '%': 7,
        '~': 8
      }
      console.log(expression)
      // 转换后缀表达式
      while (expression.length !== 0) {
        let ele = expression.shift()
        // 判断数字
        if (!isNaN(ele)) {
          temp.push(ele)
        } else {
          // 操作符
          if (stack.length === 0 || stack[stack.length - 1] === '(' || ele === '(') {
            stack.push(ele)
          } else {
            // 如果遇到')'，输出所有操作符直到遇到'('
            if (ele === ')') {
              while (stack[stack.length - 1] !== '(' && stack.length !== 0) {
                temp.push(stack.pop())
              }
              if (stack[stack.length - 1] === '(') {
                stack.pop()
              }
            } else {
              // 判断操作符的优先级
              // 如果优先级小，则继续往下直到优先级不小或者栈空为止
              while (map[ele] <= map[stack[stack.length - 1]] && stack.length !== 0) {
                if (map[stack[stack.length - 1]] === '(') {
                  break
                }
                temp.push(stack.pop())
              }
              stack.push(ele)
            }
          }
        }
      }
      while (stack.length !== 0 && stack[stack.length - 1] !== '(') {
        temp.push(stack.pop())
      }
      return temp
    },
    // 对后缀表达式进行计算
    computeExpression (temp) {
      let stack = []
      while (temp.length > 0) {
        let ele = temp.shift()
        if (!isNaN(ele)) {
          stack.push(ele)
        } else {
          let num1, num2, num3
          switch (ele) {
            case '+':
              num1 = stack.pop()
              num2 = stack.pop()
              num3 = num2 + num1
              break
            case '-':
              num1 = stack.pop()
              num2 = stack.pop()
              num3 = num2 - num1
              break
            case '*':
              num1 = stack.pop()
              num2 = stack.pop()
              num3 = num2 * num1
              break
            case '/':
              num1 = stack.pop()
              num2 = stack.pop()
              num3 = Math.floor(num2 / num1)
              break
            case '%':
              num1 = stack.pop()
              num2 = stack.pop()
              num3 = num2 % num1
              break
            case 'Lsh':
              num1 = stack.pop()
              num2 = stack.pop()
              num3 = num2 << num1
              break
            case 'Rsh':
              num1 = stack.pop()
              num2 = stack.pop()
              num3 = num2 >> num1
              break
            case '&':
              num1 = stack.pop()
              num2 = stack.pop()
              num3 = num2 & num1
              break
            case '|':
              num1 = stack.pop()
              num2 = stack.pop()
              num3 = num2 | num1
              break
            case '^':
              num1 = stack.pop()
              num2 = stack.pop()
              num3 = num2 ^ num1
              break
            default:
              break
          }
          // 每一步运算结果进行精度截取
          num3 = this.accuracyLimmit(num3)
          stack.push(num3)
          // 存储最后一步，以便进行连等
          if (temp.length === 0) {
            this.history = {
              num: num1,
              op: ele
            }
          }
        }
      }
      return stack[0]
    },
    // 计算一元操作符
    computeUnaryoperator (op, num) {
      let result
      switch (op) {
        case '~':
          result = ~num
          return result
        case 'neg':
          result = ~num + 1
          return result
        default:
          break
      }
    }
  }
}
</script>

<style lang='scss'>
@font-face {
  font-family: 'iconfont';  /* project id 1979566 */
  src: url('//at.alicdn.com/t/font_1979566_tsokmuxv2bn.eot');
  src: url('//at.alicdn.com/t/font_1979566_tsokmuxv2bn.eot?#iefix') format('embedded-opentype'),
  url('//at.alicdn.com/t/font_1979566_tsokmuxv2bn.woff2') format('woff2'),
  url('//at.alicdn.com/t/font_1979566_tsokmuxv2bn.woff') format('woff'),
  url('//at.alicdn.com/t/font_1979566_tsokmuxv2bn.ttf') format('truetype'),
  url('//at.alicdn.com/t/font_1979566_tsokmuxv2bn.svg#iconfont') format('svg');
}

.programmer {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  .programmer-system {
    flex: 1.5;
    height: 2rem;
    .programmer-system-item {
      height: 22%;
      margin-top: 0.04rem;
      border-left: 0.08rem solid transparent;
      &:hover {
        background-color: $buttonHoverColor;
      }
      .system-val {
        float: left;
        height: 100%;
        line-height: 0.45rem;
        font-size: 0.2rem;
        margin-left: 0.3rem;
      }
      .system-name {
        float: left;
        height: 100%;
        line-height: 0.45rem;
        font-size: 0.2rem;
        padding-left: 0.2rem;
        text-align: center;
        width: 0.6rem;
      }
    }
    .current-system {
      border-left: 0.08rem solid #0078d7;
    }
  }
  .programmer-menu {
    flex: 1;
    overflow: hidden;
    margin-top: 0.2rem;
    .iconfont {
      font-family: "iconfont" !important;
      font-size: 0.32rem;
      font-style: normal;
      -webkit-font-smoothing: antialiased;
      -webkit-text-stroke-width: 0.004rem;
      -moz-osx-font-smoothing: grayscale;
      border-bottom: 0.04rem solid transparent;
    }
    .current {
      color: #4599db;
      border-bottom-color: #4599db;
    }
    div {
      width: 16.2%;
      height: 0.8rem;
      line-height: 0.8rem;
      margin-right: 0.04rem;
      text-align: center;
      float: left;
      border: 0.02rem solid transparent;
      // &:hover {
      //   background-color: $buttonHoverColor;
      //   border: 0.02rem solid #a8a8a8;
      // }
    }
    .chooseSystem {
      width: 16.2%;
      margin: 0 1rem;
      font-size: 0.28rem;
      &:hover {
        background-color: $buttonHoverColor;
        border: 0.02rem solid #a8a8a8;
      }
    }
    .memory-button {
      width: 1.5rem;
      float: right;
      font-size: 0.28rem;
      height: 0.8rem;
      button {
        width: 100%;
        height: 100%;
        text-align: center;
        padding: 0;
        margin: 0;
        border: none;
        font-size: 0.28rem;
        outline: none;
        background-color: transparent;
        border: 0.02rem solid transparent;
        &:hover {
          background-color: $buttonHoverColor;
          border: 0.02rem solid #a8a8a8;
        }
      }
      button[disabled] {
        &:hover {
          background-color: transparent;
          border: 0.02rem solid transparent;
        }
      }
    }
  }
  .programmer-content {
    flex: 5;
    height: 4.8rem;
    margin-top: 0.04rem;
    .programmer-button {
      display: flex;
      flex-flow: row wrap;
      justify-content: space-between;
      align-content: flex-start;
      align-items: center;
      .programmer-button-item {
        position: relative;
        box-sizing: border-box;
        flex: 0 0 16.2%;
        height: 1rem;
        line-height: 1rem;
        text-align: center;
        font-size: 0.4rem;
        margin: 0.04rem 0;
        button {
          padding: 0;
          margin: 0;
          width: 100%;
          height: 100%;
          border: none;
          outline: none;
          position: absolute;
          left: 0;
          top: 0;
          background-color: $buttonNumColor;
          border: 0.02rem solid transparent;
          &:hover {
            background-color: $buttonHoverColor;
            border: 0.02rem solid #a8a8a8;
          }
        }
        button[disabled] {
          &:hover {
            background-color: $buttonNumColor;
            border: 0.02rem solid transparent;
          }
        }
        .operator:hover {
          background-color: rgba($color: #4599db, $alpha: 0.8);
        }
        .background {
          background-color: #eeeeee;
        }
      }
    }
  }
}
</style>
