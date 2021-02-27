<template>
  <div>
    <div style="font-family: font143;transform: translate(-999px, 0px); position:absolute;">
      .
    </div>
    <div style="font-family: font199;transform: translate(-999px, 0px);position:absolute;">
      .
    </div>
    <div style="font-family: font230;transform: translate(-999px, 0px);position:absolute;">
      .
    </div>
    <canvas id="canvas" width="500" height="500"></canvas>
  </div>
</template>
<script>
export default {
  data () {
    return {
      canvas: null,
      nameFont: '85px font199'
    }
  },
  props: {
    type: {
      require: true
    },
    hidden: {
      default: false
    }
  },
  computed: {
    nameColor () {
      if (this.type === 'M') {
        return '#A6BBCF'
      } else {
        return '#FFD1DC'
      }
    }
  },
  mounted () {
    this.canvas = document.getElementById('canvas')
    this.canvas.hidden = this.hidden
    var img = new Image()
    if (this.type === 'M') {
      img.src = '/statics/backgroundM.png'
    } else {
      img.src = '/statics/backgroundF.png'
    }
    var ctx = document.getElementById('canvas').getContext('2d')
    img.onload = function () {
      console.log(img)
      ctx.drawImage(img, 0, 0)
      console.log('draw background image done')
    }
  },
  methods: {
    check (data) {
      if (!data.avatar) {
        return '没有上传头像'
      }
      if (!data.name) {
        return '没有昵称'
      }
      if (!data.signature) {
        return '没有签名'
      }
      if (!data.voice) {
        return '没有音色'
      }
      if (data.games.length > 4) {
        console.log(data.games.length)
        return '游戏项目最多4个'
      }
      if (data.games.length === 0) {
        console.log(data.games.length)
        return '至少需要一个游戏'
      }
      if (!this.checkNameStyle(data.name)) {
        return '你输入的姓名过长，请更换一个短一些的昵称'
      }
      if (data.longSignature) {
        if (!this.checkLongSignatureStyle(data.signature)) {
          return '第一行签名过长'
        }
        if (!this.checkLongSignatureStyle(data.secondLineSignature)) {
          return '第二行签名过长'
        }
      } else {
        if (!this.checkSignatureStyle(data.signature)) {
          return '你输入的签名过长，请更换一个短一些的'
        }
      }
      if (!this.checkVoiceStyle(data.voice)) {
        return '你输入的音色过长，请更换一个短一些的'
      }
      return 'correct'
    },
    drawAvatar (data) {
      var img = new Image()
      img.src = data.avatar
      var ctx = this.canvas.getContext('2d')
      var that = this
      img.onload = function () {
        ctx.drawImage(img, 235, 272, 200, 200)
        var generatedImg = that.canvas.toDataURL('image/png')
        that.$emit('imageGenerated', generatedImg)
      }
    },
    checkNameStyle (name) {
      var ctx = this.canvas.getContext('2d')
      ctx.font = this.nameFont
      return ctx.measureText(name).width < 400
    },
    drawName (data) {
      var ctx = this.canvas.getContext('2d')
      ctx.font = this.nameFont
      ctx.fillStyle = this.nameColor
      ctx.fillText(data.name, 500 - 70 - ctx.measureText(data.name).width, 175)
    },
    checkVoiceStyle (voice) {
      return voice.length < 7
    },
    drawVoice (data) {
      var ctx = this.canvas.getContext('2d')
      ctx.font = '25px font230'
      ctx.fillStyle = '#4C4C4C'
      ctx.fillText(data.voice, 20, 100)
    },
    checkSignatureStyle (signature) {
      var ctx = this.canvas.getContext('2d')
      ctx.font = '35px font230'
      return ctx.measureText(signature).width < 400
    },
    checkLongSignatureStyle (signature) {
      var ctx = this.canvas.getContext('2d')
      ctx.font = '30px font230'
      return ctx.measureText(signature).width < 400
    },
    drawLongSignature (data) {
      var ctx = this.canvas.getContext('2d')
      ctx.textAlign = 'start'
      ctx.font = '30px font230'
      ctx.fillText(data.signature, 25, 210)
      ctx.fillText(data.secondLineSignature, 400 - ctx.measureText(data.secondLineSignature).width, 250)
    },
    drawSignature (data) {
      if (data.longSignature) {
        this.drawLongSignature(data)
        return
      }
      var ctx = this.canvas.getContext('2d')
      const x = this.canvas.width / 2
      ctx.textAlign = 'center'
      ctx.font = '35px font230'
      ctx.fillText(data.signature, x, 240)
    },
    drawGame (data) {
      if (data.games.length === 4) {
        this.drawGame4(data)
      }
      if (data.games.length === 3) {
        this.drawGame3(data)
      }
      if (data.games.length === 2) {
        this.drawGame2(data)
      }
      if (data.games.length === 1) {
        this.drawGame1(data)
      }
    },
    drawGame1 (data) {
      var ctx = this.canvas.getContext('2d')
      ctx.font = '42px font143'
      if (this.type === 'M') {
        ctx.fillStyle = '#A6BBCF'
      } else {
        ctx.fillStyle = '#FFD1DC'
      }
      ctx.textAlign = 'center'
      ctx.fillText(data.games[0], 110, 400)
    },
    drawGame2 (data) {
      var ctx = this.canvas.getContext('2d')
      ctx.font = '42px font143'
      if (this.type === 'M') {
        ctx.fillStyle = '#A6BBCF'
      } else {
        ctx.fillStyle = '#FFD1DC'
      }
      ctx.textAlign = 'start'
      ctx.fillText(data.games[0], 20, 360)
      ctx.fillStyle = '#B2B2B2'
      ctx.font = '34px font143'
      ctx.fillText(data.games[1], 500 - 270 - ctx.measureText(data.games[1]).width, 420)
    },
    drawGame3 (data) {
      var ctx = this.canvas.getContext('2d')
      ctx.font = '38px font143'
      if (this.type === 'M') {
        ctx.fillStyle = '#A6BBCF'
      } else {
        ctx.fillStyle = '#FFD1DC'
      }
      ctx.textAlign = 'start'
      ctx.fillText(data.games[0], 500 - 270 - ctx.measureText(data.games[0]).width, 300)
      ctx.fillStyle = '#B2B2B2'
      ctx.font = '34px font143'
      ctx.fillText(data.games[1], 500 - 270 - ctx.measureText(data.games[1]).width, 370)
      ctx.fillText(data.games[2], 500 - 270 - ctx.measureText(data.games[2]).width, 440)
    },
    drawGame4 (data) {
      var ctx = this.canvas.getContext('2d')
      ctx.font = '34px font143'
      if (this.type === 'M') {
        ctx.fillStyle = '#A6BBCF'
      } else {
        ctx.fillStyle = '#FFD1DC'
      }
      ctx.textAlign = 'start'
      ctx.fillText(data.games[0], 500 - 270 - ctx.measureText(data.games[0]).width, 300)
      ctx.fillStyle = '#B2B2B2'
      ctx.fillText(data.games[1], 500 - 270 - ctx.measureText(data.games[1]).width, 351)
      ctx.fillText(data.games[2], 500 - 270 - ctx.measureText(data.games[2]).width, 402)
      ctx.fillText(data.games[3], 500 - 270 - ctx.measureText(data.games[3]).width, 453)
    },
    renderCanvas (data) {
      this.drawAvatar(data)
      this.drawName(data)
      this.drawVoice(data)
      this.drawSignature(data)
      this.drawGame(data)
    }
  }
}
</script>
<style scoped>
@font-face {
  font-family: 'font143';
  src: url(../css/fonts/font143.ttf);
}
.game-font {
  font-family: 'font143';
}
@font-face {
  font-family: 'font199';
  src: url(../css/fonts/font199.ttf);
}
.signature-font {
  font-family: 'font230';
}
@font-face {
  font-family: 'font230';
  src: url(../css/fonts/font230.ttf);
}
.name-font {
  font-family: 'font199';
}
</style>
