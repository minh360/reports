<template>
  <HomePage/>
</template>

<script setup>
import HomePage from './components/HomePage.vue'
const today = new Date()
if(localStorage.getItem('time')==String(today.getDate())+String(today.getMonth() + 1)+String(today.getFullYear())){
  console.log("Same Day!!!")
}
else {
    let k = String(today.getDate())+String(today.getMonth() + 1)+String(today.getFullYear())
    localStorage.setItem('time', k)
    localStorage.removeItem('group')
}
if(localStorage.getItem('rule')==null){
  localStorage.setItem('rule',JSON.stringify(['V','M','T','C','A','S'])
  )
}
if(localStorage.getItem('ruleInfor')==null){
  localStorage.setItem('ruleInfor',JSON.stringify([
    { 
      label : 'V',
      text : 'Việt'
    },
    { 
      label : 'M',
      text : 'Mã'
    },
    { 
      label : 'T',
      text : 'Thái'
    },
    { 
      label : 'A',
      text : 'Ấn'
    },
    { 
      label : 'S',
      text : 'Singapore'
    },
    { 
      label : 'C',
      text : 'Cam'
    },
  ])
  )
}
</script>

<style lang="scss">
#app {
  font-family: "Lobster", sans-serif;
  font-weight: 400;
  font-style: normal;
  .logo{
  font-family: "Madimi One", sans-serif;
  }
  -moz-user-select: none ;
  -webkit-touch-callout: none ;
  -webkit-user-select: none ;
  -khtml-user-select: none ;
  -moz-user-select: none ;
  -ms-user-select: none ;
  user-select: none ;
}
</style>
