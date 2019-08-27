<template>
  <div id="app">
     <TodoList :items="sortedItem" @onItemDone="done"></TodoList>
     <InputFrom  @onSave="save"></InputFrom>
  </div>
</template>

<script>

import TodoList from '@/components/TodoList'
import InputFrom from '@/components/InputFrom'

export default {
  name: 'app',
  components: {
    TodoList,
    InputFrom
  },
  data (){
    return { 
      items: []
    }
    
  },
  filters: {
    caplitalize (val){
      return val.toUpperCase()
    }
  },
  mounted (){
    this.items= JSON.parse(localStorage['todoItems'])
  },
  computed: {
    sortedItem(){
      return this.items.sort((a,b) => {
        return b.time - a.time
      }).filter(ele => {
        return ele.completed === false
      })
    }
  },
  methods:{
    done(id){
      this.items = this.items.map(ele => {
        if (id === ele.time){
          ele.completed = true
        }
        return ele
      })
       localStorage['todoItems'] = JSON.stringify(this.items)
    },
    save(text){
      this.items.push({
        text: text,
        time: Date.now(),
        completed: false
      })
      localStorage['todoItems'] = JSON.stringify(this.items)
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
