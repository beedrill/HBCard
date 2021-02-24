<template>
    <div class="q-pa-md text-subtitle1" style="font-family: font199">
        <namecard-canvas :hidden="!debug" :type="type" ref="namecardCanvas" @imageGenerated="imageGenerated"/>
        <div class="text-center">
        <croppa
          :width="200"
          :height="200"
          :canvas-color="'#FBDCC6'"
          :placeholder="'请选择头像'"
          :placeholder-font-size="20"
          ref="myAvatar"
        />
        </div>
        请填写你的陪玩信息：
        <q-input class="q-my-sm" outlined v-model="name" label="昵称" />
         <q-input class="q-my-sm" outlined v-model="voice" label="音色(例如：少女音，萝莉音，御姐音)" />
         <q-input class="q-my-sm" outlined v-model="signature" label="个人签名（彰显你的特点）" />
         陪玩项目：
          <q-select
            filled
            v-model="games"
            multiple
            :options="gameOptions"
            counter
            max-values="4"
            hint="必须选4个"
            style="width: 250px"
        />
        <q-btn v-if="debug" label="预览" @click="preview"/>
    </div>
</template>
<script>
import NamecardCanvas from './NamecardCanvas.vue'
export default {
  components: { NamecardCanvas },
  props: {
    type: {}
  },
  data () {
    return {
      debug: false,
      games: [],
      gameOptions: [
        '英雄联盟', 'PUBG', 'TFT', 'COD', 'CSGO', '彩虹6号', '逃离塔科夫', 'Steam小游戏', 'DOTA2', 'Valorant', '守望先锋', 'Apex', '手游', '魔兽', '其他端游', '陪聊', '唱歌'
      ],
      voice: '',
      signature: '',
      imageUrl: '',
      name: ''
    }
  },
  mounted () {
  },
  methods: {
    imageGenerated (data) {
      // console.log(data)
      this.$emit('imageGenerated', data)
    },
    getData () {
      var imageUrl = this.$refs.myAvatar.generateDataUrl()
      return {
        name: this.name,
        games: this.games,
        voice: this.voice,
        signature: this.signature,
        avatar: imageUrl
      }
    },
    preview () {
      this.$refs.namecardCanvas.renderCanvas(this.getData())
    },
    check () {
      return this.$refs.namecardCanvas.check(this.getData())
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
  font-family: 'font143';
}
@font-face {
  font-family: 'font230';
  src: url(../css/fonts/font230.ttf);
}
</style>
