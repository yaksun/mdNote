## vuex的使用

* 1.actions.js

  ```js
  import {ADD_OPTION,PLUS_OPTION} from './mutations_type'
  export default {
  //接收和传递参数，注意{}的使用
    add_option({commit},selectOption){
      commit(ADD_OPTION,{selectOption})
    },
    plus_option({commit},selectOption){
      commit(PLUS_OPTION,{selectOption})
    },
    lazy_add({commit},selectOption){
      setTimeout(()=>{
        commit(ADD_OPTION,{selectOption})
      },2000)
    },
   // 可以使用state
    odd_add({state,commit},selectOption){
        if(state.res%2 == 0){
          commit(ADD_OPTION,{selectOption})
        }
    }
  }
  ```

* 2.mutations.js

  ```js
  import {ADD_OPTION,PLUS_OPTION} from './mutations_type'
  
  export default {
      //接收和传递参数，注意{}的使用
    [ADD_OPTION](state,{selectOption}){
      state.res = state.res + selectOption*1
    },
    [PLUS_OPTION](state,{selectOption}){
        state.res = state.res - selectOption*1
    }
  }
  ```

* 3.getters.js

  ```js
  export default {
  //可以使用state
    getType(state){
      return state.res%2 == 0 ? '偶数' : '奇数'
    }
  }
  
  ```

* 4.vue文件

  ```vue
  <template>
      <div>
        <hr>
        <select name="" id="" v-model="selectOption" >
          <option value="1">1</option>
          <option value="2">2</option>
          <option value="3">3</option>
        </select>
           <!--简洁语法-->
          <span>结果是;{{this.$store.state.res}}---当前是{{getType}}</span>
        <br> <br><br>
        <button @click="add_option(selectOption)">加</button>
        <button @click="plus_option(selectOption)">减</button>
        <button @click="lazy_add(selectOption)">延迟加</button>
        <button @click="odd_add(selectOption)">偶数加</button>
  
      </div>
  </template>
  
  <script>
     // 简洁语法
    import {mapGetters,mapActions} from 'vuex'
      export default {
          name: "MyVuex",
          data(){
            return {
              selectOption:'1',
              res:0
            }
          },
          
        computed:{
            // 简洁语法
          ...mapGetters(['getType'])
        },
        methods:{
            // 简洁语法
          ...mapActions(['add_option','plus_option','lazy_add','odd_add'])
        }
  
      }
  </script>
  ```

* 

* 

  