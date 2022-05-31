<template>
  <div id="app">
    <label>추첨자 등록</label>
    <input v-model="nameForm">
    <button @click="addItem">추가</button>
    <label>당첨자 수</label>
    <input v-model="count" />
    <button @click="hit">뽑기</button>
    <ol>
      <li v-for="target in targetList" :key="target">
        {{ target }}
      </li>
    </ol>
    <h1>
      당첨자
    </h1>
    <ul v-if="isDone" v-once>
      <li v-for="gacha in gachas" :key="gacha">
        {{ gacha }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      nameForm : '',
      count: 0,
      targetList: [],
      gachas: [],
      isDone: false
    }
  },
  methods: {
    addItem: function () {
      if(this.nameForm.indexOf(','))
      this.targetList.push(this.nameForm)
      this.nameForm = ''
    },
    hit: function() {
      let random = Date.now();

      for(let i = 0; i < 10; i++) {
        random = this.getRandom(random)
      }

      for(let i =0; i < this.count; i++) {
        let temp = this.targetList[random % this.targetList.length]
        this.gachas.push(temp)
        this.targetList = this.targetList.filter(x => x != temp)
        random = this.getRandom(random)
      }

      this.isDone = true

      // fetch('https://hooks.slack.com/services/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', {
      //   method: 'POST',
      //   cache: 'no-cache',
      //   body: JSON.stringify({
      //     text: `이번 당첨자 : ${this.gachas}`
      //   })
      // })
    },
    getRandom: function(base) {
      if(!base) {
        base = 0
      }

      let random = Number.parseInt(Math.random() * 100 * Date.now() + base) % 100000000;
      return random
    },
    
  }
}
</script>

<style>
</style>
