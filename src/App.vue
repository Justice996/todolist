<template>
  <div id="app">
      <!--监听回车事件v-on:keyup.enter-->

      <input type="text" v-model="todo" @keyup.enter="doAdd" class="new-todo" placeholder="需要做什么,按回车添加"/>
      <!--添加单击事件v-on:click，点击增加时获取文本框的值-->
      <!--<button @click="doAdd">+增加</button>-->
      <!--循环输出待办事件-->
      <h1 class="bt">正在进行</h1>
      <ul class="todo-list">
          <!--通过判断实现打钩-->
          <!--change判断是否发生改变，改变就保存一次状态-->
            <li v-for="(item,k) in list0" v-if="!item.checked" @change="saveList">
                <input type="checkbox" v-model="item.checked"> {{item.title}} --- <button @click="removeData(k)">删除</button>
            </li>
      </ul>
      <br>
      <h1 class="bt">已完成</h1>
      <ul class="todo-list">
          <li v-for="(item,k) in list0" v-if="item.checked" @change="saveList">
              <input type="checkbox" v-model="item.checked"> {{item.title}} --- <button @click="removeData(k)">删除</button>
          </li>
      </ul>
  </div>
</template>

<script>
    import stroage from './model/storage.js';
    export default {
  data () {
    return {
        todo:"",
        //定义一个空数组用来存放代办事项
        //checked用来判断是否已完成
        list0:[]
    }
  },
    methods:{
      //添加待办事项的方法
      doAdd(){
      //    获取文本框的值push到数组中
      //    this.todo获取文本框的值
      //    判断是否为空如果为空就不添加
          if(this.todo!==''){
              //添加一个标识用来区分是否完成checked
          this.list0.push({
              title:this.todo,
              checked :false
          });
          //设置缓存
              stroage.set('list',this.list0)
      //    清空文本框中的值
          this.todo='';
          }else {
              return;
          }
      },
      //删除
        removeData(k){
          this.list0.splice(k,1);
            stroage.set('list',this.list0)
        //  splice() 方法向/从数组中添加/删除项目，然后返回被删除的项目。
        // arrayObject.splice(	必需。整数，规定添加/删除项目的位置，使用负数可从数组结尾处规定位置;要删除的项目数量)
        },
        saveList(){
            //@change判断是否发生改变，改变就保存一次状态
            stroage.set('list',this.list0)
        }
    },
    //生命周期函数 vue页面刷新就会触发
    mounted(){
      let list=stroage.get('list');
      if(list){
      //    判断list是否为空
      this.list0=list;
      }
    }
}
</script>

<style lang="scss">
    * {
        margin: 0;
        padding: 0;
    }
    #app {
        font: 14px "Helvetica Neue", Helvetica, Arial, sans-serif;
        line-height: 1.4em;
        background: #f5f5f5;
        color: #4d4d4d;
        min-width: 230px;
        max-width: 550px;
        margin: 0 auto;
        padding: 0;
        -webkit-font-smoothing: antialiased;
        -moz-font-smoothing: antialiased;
        font-smoothing: antialiased;
        font-weight: 300;
    }
.new-todo {
    padding: 16px 16px 16px 60px;
    border: none;
    background: rgba(0, 0, 0, 0.003);
    box-shadow: inset 0 -2px 1px rgba(0, 0, 0, 0.03);
}
    .new-todo{
        position: relative;
        margin: 0;
        width: 100%;
        font-size: 24px;
        font-family: inherit;
        font-weight: inherit;
        line-height: 1.4em;
        border: 0;
        outline: none;
        color: inherit;
        padding: 6px;
        border: 1px solid #999;
        box-shadow: inset 0 -1px 5px 0 rgba(0, 0, 0, 0.2);
        box-sizing: border-box;
        -webkit-font-smoothing: antialiased;
        -moz-font-smoothing: antialiased;
        font-smoothing: antialiased;
    }
    .todo-list {
        margin-left: 0;
        padding: 0;
        list-style: none;
    }

    .todo-list li {
        margin-left:100px;
        position: relative;
        font-size: 24px;
        border-bottom: 1px solid #ededed;
    }

    .todo-list li:last-child {
        border-bottom: none;
    }

    #todo-list li.editing {
        border-bottom: none;
        padding: 0;
    }

    .todo-list li.editing .edit {
        display: block;
        width: 506px;
        padding: 13px 17px 12px 17px;
        margin: 0 0 0 43px;
    }

    .todo-list li.editing .view {
        display: none;
    }
   .bt{
       margin: 40px;
   }

</style>
