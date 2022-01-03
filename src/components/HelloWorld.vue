<script setup lang="ts">
import { ref, reactive } from 'vue'
import axios from 'axios'
import liff from '@line/liff'

const state = reactive({
  lang: '',
  version: '',
  client: false,
  isLoggedin: false,
  os: null,
  lineVersion: ''
})

defineProps<{ msg: string }>()
liff.init({
  liffId: `${import.meta.env.VITE_LIFF_ID}`
}).then(() => {
  state.lang = liff.getLanguage()
  state.version = liff.getVersion()
  state.client = liff.isInClient()
  state.isLoggedin = liff.isLoggedIn()
  state.os = liff.getOS() as any
  state.lineVersion = liff.getLineVersion() as any
})

const setRichMenu = () => {
  axios.post('https://api.line.me/v2/bot/richmenu', {
    headers: {
      Authorization: `Bearer ${import.meta.env.VITE_LINE_KEY}`,
      'Content-Type': 'application/json'
    },
    withCredentials: true
  })
}

const count = ref(0)
</script>

<template>
  <h1>{{ msg }}</h1>
  <button @click="setRichMenu">リッチメニュー作成</button>
  <div>
    {{ state.lang }}
    {{ state.version }}
    {{ state.client }}
    {{ state.isLoggedin }}
    {{ state.os }}
    {{ state.lineVersion }}
  </div>
</template>

<style scoped>
a {
  color: #42b983;
}

label {
  margin: 0 0.5em;
  font-weight: bold;
}

code {
  background-color: #eee;
  padding: 2px 4px;
  border-radius: 4px;
  color: #304455;
}
</style>
