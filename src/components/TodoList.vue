<template>
  <div class="container-box" :class="themeClass">
    <div class="background-wrap">
      
    </div>
    <div class="todo-wrap">
      <div class="todo-inner">
        <div class="header-theme">
          <h1 class="header">TODO</h1>
          <img :src="require('@/assets/images/'+ toggleBtn)" alt="" class="theme-toggle" @click="themeToggle">
        </div>
        <div class="input-box">
          <div>
            <img src="@/assets/images/icon-check.svg" alt="">
          </div>
          <input type="text" placeholder="Create a new todo..." v-model="todoModal" class="todo-input" @keyup.enter="addTodos">
        </div>
        <div class="list-box">
          <div class="" v-if="todos.length >= 1">
            <li class="tasks" v-for="(todo,index) in todos" :key="index">
              <div :class="checkbox ? 'checkbox' : ''" @click="checked">
                <img src="@/assets/images/icon-check.svg" alt="">
              </div>
              <span class="">{{ todo.todoText }}</span>
              <img src="@/assets/images/icon-cross.svg" class="remove-btn" alt="" @click="removeList(index)">
            </li>
          </div>
          <div class="empty-box" v-else>
            <p class="">No tasks to be done</p>
          </div>
          <div class="control-bar">
            <div class="">{{ todoLeft }} items left</div>
            <div class="">
              <span @click="showAll" >All</span>
              <span @click="showActive">Active</span>
              <span @click="showComplete">Completed</span>
            </div>
            <div class="clear">
              Clear Completed
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
    data(){
      return{
        checkbox : false,
        toggleLight : true,
        toggleBtn : 'icon-sun.svg',
        themeClass : 'light',
        todoModal : '',
        todosLeft : 0,
        todos : []
      }
    },
    methods : {
      checked(){
        this.checkbox = !this.checkbox
      },

      themeToggle(){
        this.toggleLight = !this.toggleLight
        if(this.toggleLight){
          this.toggleBtn = 'icon-sun.svg'
          this.themeClass = 'light'
        }else{
          this.toggleBtn = 'icon-moon.svg'
          this.themeClass = 'dark'
        }
      },

      addTodos(){
        if(this.todoModal != ''){
          this.todos.push(
            {
              todoText : this.todoModal,
              active : true,
              complete : false
            }
          )
          this.todoModal = ''
        }
      },

      removeList(index){
        this.todos.splice(index,1)
      }
    },
    computed : {
      todoLeft(){
        return this.todos.filter((item)=> item.active = true).length
      }
    }

}
</script>

<style lang="scss">
.light{
  width : 100%;
  height : 100vh;
  background-color: hsl(0, 0%, 98%);
  position : relative;
  transition: all 0.5s ease-in-out;

  .background-wrap{
    width : 100%;
    height : 40%;
    background-image: url("@/assets/images/bg-desktop-light.jpg");
    position : absolute;
    top : 0;
    left : 0;
    
  }

  .todo-wrap{
    width : 100%;
    height : 100%;
    position : relative;
    z-index : 2;
    display: flex;
    justify-content: center;
    align-items: center;
    
    .todo-inner{
      width : 100%;
      max-width : 520px;
      padding : 10px;

      .header-theme{
        display: flex;
        justify-content: space-between;
        align-items : center;
        margin-bottom : 20px;

        .header{
          color : white;
          letter-spacing : 10px;
        }
        .img{
          user-select: none;
        }
      }

      .input-box{
        background-color: white;
        padding : 20px;
        border-radius: 8px;
        display: flex;
        align-items: center;
        box-shadow : 5px 5px 30px hsl(233, 11%, 84%);

        div{
          display : flex;
          justify-content: center;
          align-items : center;
          width : 23px;
          height : 23px;
          background-image: none;
          padding : 5px;
          border: 1px solid hsl(236, 33%, 92%);
          border-radius: 50%;
          margin-right : 10px;
          user-select: none;

          img{
            display: none;
          }
        }
        div.checkbox{
          background-image: linear-gradient(hsl(192, 100%, 67%),hsl(280, 87%, 65%));

          img{
            display : block;
          }
        }

        .todo-input{
          border : none;
          font-size : 16px;

          &:focus{
            outline : none;
          }
        }
      }

      .list-box{
        background-color: white;
        border-radius: 8px;
        margin-top: 30px;
        box-shadow : 5px 5px 30px hsl(233, 11%, 84%);

        .empty-box{
          display : flex;
          justify-content : center;
          padding : 20px;
          
          p{
            color : hsl(235, 19%, 35%);
            font-size : 16px;
          }
        }

        .tasks{
          display : flex;
          align-items : center;
          width: 100%;
          border-bottom : 1px solid hsl(236, 33%, 92%);
          padding : 20px;

          &:hover{
            .remove-btn{
              display : block;
            }
          }

          div{
            cursor: pointer;
            display : flex;
            justify-content: center;
            align-items : center;
            width : 23px;
            height : 23px;
            background-image: none;
            padding : 5px;
            border: 1px solid hsl(236, 33%, 92%);
            border-radius: 50%;
            margin-right : 10px;
            user-select: none;

            &:hover{
              border-color : hsl(236, 33%, 62%);
            }

            img{
              display: none;
            }
          }
          div.checkbox{
            background-image: linear-gradient(hsl(192, 100%, 67%),hsl(280, 87%, 65%));

            img{
              display : block;
            }
          }

          span{
            color : hsl(235, 19%, 35%);
            cursor : pointer;
          }

          .remove-btn{
            cursor : pointer;
            width: 18px;
            height: 18px;
            margin-left : auto;
            display : none;
            transition : display 1s ease;
          }
        }

        .control-bar{
          display : flex;
          justify-content: space-between;
          align-items : center;
          color : hsl(236, 9%, 61%);
          font-size: 14px;
          font-weight : 500;
          padding : 15px 20px;

          div span{
            margin-right : 15px;
            cursor: pointer;

            &:hover{
              color : hsl(235, 19%, 35%);
            }
          }

          .clear{
            cursor : pointer;
            
            &:hover{
              color: hsl(236, 9%, 41%);
            }
          }
        }
      }
    }
  }
}

.dark{
  width : 100%;
  height : 100vh;
  background-color: hsl(235, 21%, 11%);
  position : relative;
  transition: all 0.5s ease-in-out;

  .background-wrap{
    width : 100%;
    height : 40%;
    background-image: url("@/assets/images/bg-desktop-dark.jpg");
    position : absolute;
    top : 0;
    left : 0;
    
  }

  .todo-wrap{
    width : 100%;
    height : 100%;
    position : relative;
    z-index : 2;
    display: flex;
    justify-content: center;
    align-items: center;
    
    .todo-inner{
      width : 100%;
      max-width : 520px;
      padding : 10px;

      .header-theme{
        display: flex;
        justify-content: space-between;
        align-items : center;
        margin-bottom : 20px;

        .header{
          color : white;
          letter-spacing : 10px;
        }
        .img{
          user-select: none;
        }
      }

      .input-box{
        background-color: hsl(237, 14%, 26%);
        padding : 20px;
        border-radius: 8px;
        display: flex;
        align-items: center;
        // box-shadow : 5px 5px 30px hsl(233, 11%, 84%);

        div{
          display : flex;
          justify-content: center;
          align-items : center;
          width : 23px;
          height : 23px;
          background-image: none;
          padding : 5px;
          border: 1px solid hsl(237, 14%, 26%);
          border-radius: 50%;
          margin-right : 10px;
          user-select: none;

          img{
            display: none;
          }
        }
        div.checkbox{
          background-image: linear-gradient(hsl(192, 100%, 67%),hsl(280, 87%, 65%));

          img{
            display : block;
          }
        }

        .todo-input{
          border : none;
          font-size : 16px;
          background-color: hsl(237, 14%, 26%);
          color: hsl(234, 39%, 85%);

          &:focus{
            outline : none;
          }
        }
      }

      .list-box{
        background-color: hsl(237, 14%, 26%);
        border-radius: 8px;
        margin-top: 30px;
        // box-shadow : 5px 5px 30px hsl(233, 11%, 84%);

        .empty-box{
          display : flex;
          justify-content : center;
          padding : 20px;
          
          p{
            color : hsl(236, 33%, 92%);
            font-size : 16px;
          }
        }

        .tasks{
          display : flex;
          align-items : center;
          width: 100%;
          border-bottom : 1px solid hsl(236, 9%, 50%);
          padding : 20px;

          &:hover{
            .remove-btn{
              display : block;
            }
          }

          div{
            cursor: pointer;
            display : flex;
            justify-content: center;
            align-items : center;
            width : 23px;
            height : 23px;
            background-image: none;
            padding : 5px;
            border: 1px solid hsl(236, 9%, 50%);
            border-radius: 50%;
            margin-right : 10px;
            user-select: none;

            &:hover{
              border-color : hsl(236, 33%, 92%);
            }

            img{
              display: none;
              
            }
          }
          div.checkbox{
            background-image: linear-gradient(hsl(192, 100%, 67%),hsl(280, 87%, 65%));

            img{
              display : block;
            }
          }

          span{
            color : hsl(236, 33%, 92%);
            cursor : pointer;
          }

          .remove-btn{
            cursor : pointer;
            width: 18px;
            height: 18px;
            margin-left : auto;
            display : none;
            transition : display 1s ease;
            filter: invert(97%) sepia(3%) saturate(3054%) hue-rotate(187deg) brightness(101%) contrast(89%);
          }
        }

        .control-bar{
          display : flex;
          justify-content: space-between;
          align-items : center;
          color : hsl(236, 9%, 61%);
          font-size: 14px;
          font-weight : 500;
          padding : 15px 20px;

          div span{
            margin-right : 15px;
            cursor: pointer;

            &:hover{
              color : hsl(236, 33%, 92%);
            }
          }

          .clear{
            cursor : pointer;
            
            &:hover{
              color: hsl(236, 33%, 92%);
            }
          }
        }
      }
    }
  }
}
</style>