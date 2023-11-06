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
            <li class="tasks" v-for="(todo,index) in todos" :key="index" :class="{throughLine : todo.complete}">
              <div :class="todo.complete ? 'checkbox' : ''" @click="checked(index)">
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
            <div class="" v-if="!mobileDisplay">
              <span @click="showAll" :class="currentList == 'all' ? 'active' : ''">All</span>
              <span @click="showActive" :class="currentList == 'active' ? 'active' : ''">Active</span>
              <span @click="showComplete" :class="currentList == 'completed' ? 'active' : ''">Completed</span>
            </div>
            <div class="clear" @click="removeCompleteItems">
              Clear Completed
            </div>
          </div>
        </div>
        <div class="mobile-control" v-if="mobileDisplay">
              <span @click="showAll" :class="currentList == 'all' ? 'active' : ''">All</span>
              <span @click="showActive" :class="currentList == 'active' ? 'active' : ''">Active</span>
              <span @click="showComplete" :class="currentList == 'completed' ? 'active' : ''">Completed</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
    data(){
      return{
        checkbox : null,
        toggleLight : true,
        toggleBtn : 'icon-sun.svg',
        themeClass : 'light',
        todoModal : '',
        todosLeft : 0,
        currentList : '',
        todos : [],
        todoLists : [],
        mobileDisplay : null,
      }
    },
    methods : {
      checked(index){
        this.todoLists[index].active = !this.todoLists[index].active
        this.todoLists[index].complete = !this.todoLists[index].complete
        this.todos = this.todoLists
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
          this.todoLists.push(
            {
              todoText : this.todoModal,
              active : true,
              complete : false
            }
          )
          this.todos = this.todoLists
          this.todoModal = ''
        }
      },

      removeList(index){
        this.todoLists.splice(index,1)
        this.todos = this.todoLists
      },

      showAll(){
        this.currentList = 'all'
        this.todos = this.todoLists
      },
      showActive(){
        this.currentList = 'active'
        this.todos = this.todoLists.filter((item) => item.active == true)
      },
      showComplete(){
        this.currentList = 'completed'
        this.todos = this.todoLists.filter((item) => item.complete == true)
      },
      removeCompleteItems(){
        this.todoLists = this.todoLists.filter((item)=> item.complete == false)
        this.todos = this.todoLists
      }
    },
    computed : {
      todoLeft(){
        return this.todoLists.filter((item)=> item.active == true).length
      }
    },
    created(){
      if(window.innerWidth <= 375){
        this.mobileDisplay = true
      }else{
        this.mobileDisplay = false
      }

      window.addEventListener('resize',()=>{
        if(window.innerWidth <= 375){
          this.mobileDisplay = true
        }else{
          this.mobileDisplay = false
        }
      })
    },
    mounted(){
      this.showAll()
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
    background-repeat: no-repeat;
    position : absolute;
    top : 0;
    left : 0;
    object-fit: cover;

    @media(max-width: 375px){
      background-image: url("@/assets/images/bg-mobile-light.jpg");
    }
    
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

      @media(max-width: 375px){
        padding : 20px;
      }

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

        .throughLine{
          text-decoration: line-through;
          color : hsl(236, 9%, 61%);

          span{
            color : hsl(236, 9%, 61%);
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

          span{
            margin-right : 15px;
            cursor: pointer;

            &:hover{
              color : hsl(235, 19%, 35%);
            }
          }

          .active{
            color : hsl(192, 80%, 57%);
            font-weight : 600;

            &:hover{
              color : hsl(192, 80%, 57%);
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

      .mobile-control{
        margin-top : 20px;
        background-color: white;
        border-radius : 8px;
        padding : 20px;
        display: flex;
        justify-content: center;
        color : hsl(236, 9%, 61%);
        font-size: 14px;
        font-weight : 500;
        box-shadow : 5px 5px 30px hsl(233, 11%, 84%);

        span{
            margin-right : 15px;
            cursor: pointer;

            &:hover{
              color : hsl(235, 19%, 35%);
            }
          }

        .active{
          color : hsl(192, 80%, 57%);
          font-weight : 600;

          &:hover{
            color : hsl(192, 80%, 57%);
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
    background-repeat: no-repeat;
    object-fit: cover;
    
    @media(max-width: 375px){
      background-image: url("@/assets/images/bg-mobile-dark.jpg");
    }
    
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

      @media(max-width: 375px){
        padding : 20px;
      }

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
          border: 1px solid hsl(236, 9%, 50%);
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

        .throughLine{
          text-decoration: line-through;
          color : hsl(236, 9%, 61%);

          span{
            color : hsl(236, 9%, 61%);
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

          span{
            margin-right : 15px;
            cursor: pointer;

            &:hover{
              color : hsl(236, 33%, 92%);
            }
          }

          .active{
            color : hsl(192, 80%, 57%);
            font-weight : 600;

            &:hover{
              color : hsl(192, 80%, 57%);
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

      .mobile-control{
        margin-top : 20px;
        background-color: hsl(237, 14%, 26%);
        border-radius : 8px;
        padding : 20px;
        display: flex;
        justify-content: center;
        color : hsl(234, 39%, 85%);
        font-size: 14px;
        font-weight : 500;
        box-shadow : 5px 5px 30px hsl(237, 14%, 16%);

        span{
            margin-right : 15px;
            cursor: pointer;

            &:hover{
              color : hsl(236, 33%, 92%);
            }
          }

        .active{
          color : hsl(192, 80%, 57%);
          font-weight : 600;

          &:hover{
            color : hsl(192, 80%, 57%);
          }
        }
      }
    }
  }
}
</style>