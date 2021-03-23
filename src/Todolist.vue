<template>
    <div class="content">
        <header>
          <section>
            <label>ToDoList</label>
            <input type="text" @keydown.enter="adding" placeholder="添加ToDo" v-model="content">
          </section>
        </header>
        <section>
            <h2>正在进行
              <span>{{todocount}}</span>
            </h2>
            <div>
                <ol>
                  <li v-for="(item, index) in list" :key="index" v-if="item.istrue">
                    <input type="checkbox" @click="added(item,false)" />
                    {{item.content}}
                    <a href="#" @click="deleted(index)"></a>
                  </li>
                </ol>
            </div>
            <h2>已经完成
              <span>{{donecount}}</span>
            </h2>
             <div>
                <ul>
                  <li v-for="(item, index) in list" :key="index" v-if="!item.istrue">
                    <input type="checkbox" @click="added(item,true)" checked />
                    {{item.content}} 
                    <a href="#" @click="deleted(index)"></a>
                  </li>
                </ul>
            </div>
        </section>
        <footer>
          Copyright &copy; 2014 todolist.cn <a href="#" @click="deleteall">clear</a>
        </footer>
    </div>
</template>
<script>
export default {
    data () {
        return {
            list : [], //定义初始数据
            content : '',//双向数据绑定 
        }
    },
    created () {  //判断是否有无缓存 如果有缓存 将其解析成json对象 
      let list = localStorage.list
      if (list) {
        this.list = JSON.parse(list)
      }
    },
    methods: {
        // 点击添加时 更行数组list，加入其文本以及是否正在进行 默认为true
        adding () {
            this.list.push ({
                content: this.content,
                istrue : true  //正在进行
              })
            this.content='', //将输入框的文本清空
            this.addStorage()  //将状态等存入缓存 避免用户刷新丢失数据
        },
        // 点击完成 未完成任务 第二个参数 true false
        added (item,state) {
            console.log(item);
            if (state) {
              item.istrue = true
            } else {
              item.istrue = false
            }
            this.finish = item.title
            this.addStorage()
        },
        deleted (index) {
            this.list.splice(index, 1)
            this.addStorage()
        },
        deleteall () {
          this.list = []
          this.addStorage()
        },
        addStorage(){  //存入缓存 并将数组序列化成json字符串
          localStorage.list =  JSON.stringify(this.list)
        },
    },
  computed: {
    todocount () {
      let num = 0
      this.list.map(item => {
        if (item.istrue) {
          num ++
        }
      })
      return num
    },
    donecount () {
      let num = 0
      this.list.map(item => {
        if (!item.istrue) {
          num ++
        }
      })
      return num
    }
  }
}
</script>

<style>
  body {
    margin: 0;
    padding: 0;
    font-size: 16px;
    background: #CDCDCD;
  }
  a {
    text-decoration: none;
  }
  ol, ul {
    list-style: none;
    padding: 0;
  }
  header {
    height: 50px;
    padding: 0 10px;
    background: rgba(47,47,47,0.98)
  }
  section {
    width: 600px;
    padding: 0 10px;
    margin: 0 auto;
  }
  label {
    float: left;
    width: 100px;
    margin-left: 10%;
    line-height: 50px;
    color: #DDD;
    font-size: 24px;
    cursor: pointer;
    font-family: "Helvetica Neue",Helvetica,Arial,sans-serif;
  }
  header input {
    float: right;
    width: 60%;
    height: 24px;
    margin-top: 12px;
    text-indent: 10px;
    border-radius: 5px;
    box-shadow: 0 1px 0 rgba(255,255,255,0.24), 0 1px 6px rgba(0,0,0,0.45) inset;
    border: none;
  }
  h2 {
    position: relative;
  }
  h2 span {
    position: absolute;
    top: 2px;
    right: 5px;
    display: inline-block;
    padding: 0 5px;
    height: 20px;
    border-radius: 20px;
    background: #E6E6FA;
    line-height: 22px;
    text-align: center;
    color: #666;
    font-size: 14px;
  }
  li {
    height: 32px;
    line-height: 32px;
    background: #fff;
    position: relative;
    margin-bottom: 10px;
    padding: 0 45px;
    border-radius: 3px;
    border-left: 5px solid #629A9C;
    box-shadow: 0 1px 2px rgba(0,0,0,0.07);
  }
  li input {
    position: absolute;
    top: 2px;
    left: 10px;
    width: 22px;
    height: 22px;
    cursor: pointer;
  }
  li a {
    position: absolute;
    top: 2px;
    right: 5px;
    display: inline-block;
    width: 14px;
    height: 12px;
    border-radius: 14px;
    border: 6px double #FFF;
    background: #CCC;
    line-height: 14px;
    text-align: center;
    color: #FFF;
    font-weight: bold;
    font-size: 14px;
    cursor: pointer;
  }
  ul li {
    opacity: 0.5;
  }
  footer {
    color: #666;
    font-size: 14px;
    text-align: center;
  }
</style>
