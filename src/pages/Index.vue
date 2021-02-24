<template>
  <q-page class="flex" :class="dynamicClass">
    <q-card style="max-width:600px" bordered flat>
    <q-stepper
      v-model="step"
      header-nav
      ref="stepper"
      color="deep-orange"
      animated
      active-color="deep-orange"
      done-color="deep-orange"
    >
      <q-step
        :name="1"
        title="选择名片类型"
        icon="settings"
        :done="done1"
        :header-nav="step > 1"
      >
        <div class="q-pa-md text-subtitle1">
          <q-option-group
            :options="typeOptions"
            label="Notifications"
            type="radio"
            v-model="type"
          />
        </div>

        <q-stepper-navigation>
          <q-btn @click="continue1"  color="deep-orange-7" unelevated label="下一步" />
        </q-stepper-navigation>
      </q-step>

      <q-step
        :name="2"
        title="陪玩信息"
        icon="create_new_folder"
        :done="done2"
        :header-nav="step > 2"
      >
        <DetailForm :type="type" ref="detailForm" @imageGenerated="imageGenerated"/>
        <q-stepper-navigation>
          <q-btn unelevated @click="continue2" color="deep-orange-7" label="Continue" />
          <q-btn flat @click="step = 1" color="black" label="Back" class="q-ml-sm" />
        </q-stepper-navigation>
      </q-step>

      <q-step
        :name="3"
        title="生成名片"
        icon="add_comment"
        :done="done3"
        :header-nav="step > 3"
      >
        <div>您可以点击下方的下载按钮下载</div>
        <q-card bordered flat>
          <q-img :src="generatedCard"/>
        </q-card>
        <q-stepper-navigation>
          <q-btn unelevated color="deep-orange" @click="download" label="下载" />
          <q-btn flat @click="step = 2" color="grey-7" label="返回" class="q-ml-sm" />
        </q-stepper-navigation>
      </q-step>
    </q-stepper>
    </q-card>
  </q-page>
</template>

<script>
import DetailForm from '../components/DetailForm'
export default {
  name: 'PageIndex',
  components: {
    DetailForm
  },
  data () {
    return {
      step: 1,
      typeOptions: [
        { label: '男孩子', value: 'M', color: 'deep-green' },
        { label: '女孩子', value: 'F', color: 'pink-9' }
      ],
      done1: false,
      done2: false,
      done3: false,
      type: '',
      generatedCard: null
    }
  },
  computed: {
    dynamicClass () {
      var tmp = {}
      if (!this.$q.platform.is.mobile) {
        tmp['flex-center'] = true
      }
      return tmp
    }
  },
  methods: {
    continue1 () {
      if (!this.type) {
        this.$q.notify({
          message: '请选择名片类型',
          color: 'red'
        })
        return
      }
      this.done1 = true
      this.step++
    },
    continue2 () {
      // var data = this.$refs.detailForm.getData()
      var checkString = this.$refs.detailForm.check()
      if (checkString !== 'correct') {
        this.$q.notify({
          message: checkString,
          color: 'red'
        })
        return
      }
      this.namecardUrl = this.$refs.detailForm.preview()
    },
    imageGenerated (data) {
      console.log('hhh ')
      this.generatedCard = data
      this.done2 = true
      this.step++
    },
    finish () {
      this.done1 = this.done2 = this.done3 = false
      this.data = null
      this.step = 1
    },
    preview () {
      this.$refs.finishCanvas.renderCanvas(this.data)
    },
    download () {
      this.downloadURI(this.generatedCard, '名片')
    },
    downloadURI (uri, name) {
      var link = document.createElement('a')
      link.download = name
      link.href = uri
      document.body.appendChild(link)
      link.click()
      document.body.removeChild(link)
    }
  }
}
</script>
