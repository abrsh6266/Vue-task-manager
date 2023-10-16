<template>
    <header>
        <h1>{{title}}</h1>
        <Button @btn-click  ="$emit('toggle-add-task')" 
        :text="showAddTask? 'Close' : 'Add Task'" 
        :color="showAddTask? 'red' : 'green'" 
        v-show="homePage"
        />
    </header>
</template>
<script>
import Button from "./Button"
export default{
    name: 'Header' , 
    props:{
        title: String,
        showAddTask:Boolean
    },
    components: {
    Button,
  },
  computed:{
    homePage(){
        if(this.$route.path==='/'){
            return true
        }else return false
    }
  }
}
</script>
<Style scoped>
    header {
        display: flex;
        justify-conent: space-between;
        align-items: center;
        margin-bottom: 20px
    }
</Style>