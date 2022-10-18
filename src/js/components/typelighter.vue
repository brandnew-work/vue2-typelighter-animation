<template>
  <div class="typelighter-text" v-text="returnText + randText"></div>
</template>

<style>
  .typelighter-text {
    font-family: sans-serif;
    font-weight: bold;
    letter-spacing: .3rem;
  }
</style>

<script>
  export default {
    props: {
      text: {
        type: String,
        default: 'Hello World',
        require: true
      },
    },
    data() {
      return {
        returnText: '',
        randText: ''
      }
    },
    mounted() {
      this.printText()
    },
    methods: {

      // 乱数の取得
      getRandom() {
        let randStr = "abcdefghijklmnopqrstuvwxyz0123456789<>$&!'()%[]"
        let randAry = [...randStr]
        let randNum = Math.floor(Math.random() * (randAry.length - 1))
        return randAry[randNum]
      },

      // 文字出力アニメーションの設定
      returnStr(text) {
        let cnt    = 0
        let cntMax = 3
        return new Promise((resolve) => {
          setInterval(() => {
            if (cnt == cntMax) {
                resolve(text)
            } else {
              this.randText = this.getRandom()
              cnt ++
            }
          }, 25)
        })
      },

      // アニメーションループの作成
      async printText() {
        for await (let text of [...this.text]) {
          this.returnText += await this.returnStr(text)
        }
        this.randText = ''
      },
    },
  }
</script>
