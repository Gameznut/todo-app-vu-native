<template>
  <view  class="container" >
    <status-bar
            background-color="hotpink"
            bar-style="light-content"
    />
    <text class="text">TODO APP</text>
    <view class="input-container">
      <text-input class="input" v-model="text"/>
      <touchable-opacity class="btn" :on-press="Add">
        <text class="btn-text">Add</text>
      </touchable-opacity>
    </view>  
    <view class="list-content" v-for="todo in todos" :key="todo.id">
      <touchable-opacity class="list-container" :on-press="() => done(todo.id)">
        <text class="lists-text done" v-if="todo.done" > {{ todo.title }} </text>
        <text class="lists-text" v-else > {{ todo.title }} </text>
      </touchable-opacity>
      <touchable-opacity class="remove-btn" :on-press="() => remove(todo.id)">
        <text class="remove-btn-text">Remove</text>
      </touchable-opacity>
    </view>
  </view>
</template>
<script>
import { AsyncStorage } from 'react-native'
export default {
  data() {
    return {
      value: "",
      text: "",
      todos: [
        {
          id: 1,
          title: "Going to the shop",
          done: false
        },
        {
          id: 2,
          title: "Going to do laudry",
          done: false
        },
      ],
      storeData: "",
      cloked: ''
    }
  },
    methods: {
      onPressButton() {
        alert(this.cloked = announceTime())
      },
        Add() {
          if (this.text.length > 0){
            this.todos.push({
              id: this.todos.length + 1,
              title: this.text,
              done: false
            })
          }
          this.text = ''
        },
        done(id){
          this.todos = this.todos.map(todo =>{
            if(todo.id == id) todo.done = !todo.done
            return todo
          })
        },
        remove(id){
          this.todos = this.todos.filter(todo => todo.id !== id)
        }
    },
  mounted() {
  this.storeData = async (todos) => {
    try {
      const jsonValue = JSON.stringify(todos)
      await AsyncStorage.setItem('@storage_Key', jsonValue)
    } catch (e) {
      // saving error
      alert('error')
    }
  }  
},
}
</script>

<style>
.container {
  background-color: rgb(62, 34, 126);
  flex: 1;
}
.text {
  color: rgb(12, 198, 231);
  font-size: 45px;
  text-align: center;
  background-color: rgb(62, 34, 126)
}
.btn{
  border-top-right-radius: 5px;
  border-bottom-right-radius: 5px;
  border-right-width: 2px;
  border-top-width: 2px;
  border-bottom-width: 2px;
  border-color: white;
  background-color: rgb(21, 173, 72);
  height: 40px;
  width: 25%;
  justify-content: center;
}
.btn-text{
  text-align: center;
  color: white;
}
.input{
  background-color : rgb(84, 106, 177);
  font-size: 20px;
  font-weight: 300;
  font-family: serif;
  width: 75%;
  height: 40px;
  border-color: white;
  border-top-left-radius: 5px;
  border-bottom-left-radius: 5px;
  border-left-width: 2px;
  border-top-width: 2px;
  border-bottom-width: 2px;
  padding-left: 6px;  
}
.input-container{
  flex-direction: row;
  justify-content: center;
  align-items: stretch;
}
.list-container{
  justify-content: center;
  width: 75%;
}
.lists-text{
  font-size: 20px;
  color: deeppink;
}
.list-content{
  flex-direction: row;
  justify-content: space-between;
  padding: 10px;
}
.done{
  color: rgb(109, 106, 102);
}
.remove-btn{
  width: 29%;
  justify-content: center;
}
.remove-btn-text{
  color: crimson;
  text-align: center;
  font-size: 20px;
}
</style>
