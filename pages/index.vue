<template>
  <section class="container">
    <div class="form" v-if="!showTable">
      <el-progress 
        :text-inside="true" 
        :stroke-width="18" 
        :percentage="progressWidth.width"
      ></el-progress>
      <div 
        class="input__wrap" 
        v-for="(el, idx) in info"
        :key="idx"
      >
        <div class="alert__wrap">
          <p class="field__text">{{ el.name }}</p>
          <el-alert
            v-if="controls[idx].activated"
            title=""
            :type="!controls[idx].error ? 'success' : 'error'"
            show-icon>
          </el-alert>
        </div>
        <el-input 
          placeholder="Please input" 
          :value="el.value"
          @input="onInput(idx, $event)"
        ></el-input>
      </div>
      <el-button 
        :disabled="done < info.length" 
        class="send__data" 
        @click="showData" 
        type="primary" 
        round
      >Primary</el-button>
    </div>
    <ShowingData :fields="info" v-if="showTable"/>
  </section>
</template>
  
<script>
import ShowingData from '@/components/Show/ShowingData'

export default {
  components: {
    ShowingData
  },
  data() {
    return {
      info: [
        {
          name: 'Name',
          value: '',
          pattern: /^[a-zA-Z ]{2,30}$/
        },
        {
          name: 'Phone',
          value: '',
          pattern: /^[\+]?[(]?[0-9]{3}[)]?[-\s\.]?[0-9]{3}[-\s\.]?[0-9]{4,6}$/im
        },
        {
          name: 'Email',
          value: '',
          pattern: /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
        },
        {
          name: 'Some Field 1',
          value: '',
          pattern: /.+/
        },
        {
          name: 'Some Fiels 2',
          value: '',
          pattern: /.+/
        }
      ],
      controls: [],
      showTable: false
    }
  }, 
  created() {
    for (let i = 0; i < this.info.length; i++) {
      this.controls.push({
        error: true,
        activated: false
      })
    }
  },
  methods: {
    onInput(idx, value) {
      let data = this.info[idx], control = this.controls[idx];

      data.value = value
      control.error = !data.pattern.test(value)
      control.activated = true

      console.log('------------------------------------');
      console.log('value', value);
      console.log('controls', this.controls);
      console.log('------------------------------------');
    },
    showData() {
      this.showTable = !this.showTable
      this.$notify({
        title: 'Success',
        message: 'This is your data',
        type: 'success'
      });
    }
  },
  computed: {
    done() {
      let done = 0

      for (let i = 0; i < this.controls.length; i++) {
        if (!this.controls[i].error) done++
      }
      return done 
    },
    progressWidth() {
      return {
        width: 100 / this.info.length * this.done
      }
    }
  }
}
</script>

<style lang="scss">
// elementary classes
.el-progress-bar__inner {
  transition: width .2s;
}
.el-progress {
  margin-bottom: 30px;
}
.el-alert {
  width: fit-content  ;
}
.el-alert--error,
.el-alert--success  {
    background-color: transparent;
}
.el-alert__content {
  display: none;
}
// my classes
.input__wrap {
  display: flex;
  flex-direction: column;
  width: 300px;
  margin-bottom: 10px;
}
.alert__wrap {
  display: flex;
  align-items: center;
  padding-left: 5px;
  font-weight: 700;
}
.field__text {
  min-height: 26px;
  display: flex;
  align-items: center;
}
.send__data {
  margin-top: 20px;
}
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  padding-top: 100px;
}
.form {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
}
.el-progress-bar__outer {
  width: 500px;
}
.title
{
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; /* 1 */
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}
.subtitle
{
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}
.links
{
  padding-top: 15px;
}
</style>
