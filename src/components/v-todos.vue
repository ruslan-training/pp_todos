<template>
  <div class="todos">
    <div class="todos__input-box">
      <div class="todos__input-wrapper">
        <input type="text" class="todos__input" placeholder="Введите Задачу"
          v-model="inputValue"
          @keypress.enter="addTask"
        >
        <button 
          type="submit" class="remove-btn"
          @click="clearInput"
        >
        </button>
      </div>
      <button type="submit" class="add-btn btn"
        @click="addTask()"
      >Добавить</button>
    </div>
    <hr class="todos__line">

    <ul class="todos__list"
     v-if="tasks.length"
    >
      <li class="todos__item"
        v-for="(task, i) in tasks"
        :key="task.id"
      > 
        <button type="submit" 
          class="todos__icon-mark"
          :class="{'active': task.isCompleted}"
          @click="completeTask(i)"

        >
          <svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="24" height="24" viewBox="0 0 1024 1024">
            <title/>
            <g id="check-img">
            </g>
            <path d="M698.24 416.96l-32.064-32.064c-5.92-5.92-15.488-5.92-21.376 0l-192.448 192.416-91.488-92.224c-5.952-5.92-15.488-5.92-21.44 0l-32.064 32.096c-5.92 5.92-5.92 15.488 0 21.376l134.176 135.136c5.92 5.888 15.488 5.888 21.376 0l235.328-235.328c5.952-5.92 5.952-15.488 0-21.408zM512 96c-229.76 0-416 186.24-416 416s186.24 416 416 416 416-186.24 416-416-186.24-416-416-416zM512 832c-176.704 0-320-143.296-320-320 0-176.736 143.296-320 320-320 176.736 0 320 143.264 320 320 0 176.704-143.264 320-320 320z"/>
          </svg>
        </button>
        <p>
          {{ task.text }}
        </p> 

         <button class="todos__item-btn item-btn btn"
                 @click="removeTask(task.id)"
                 
         >
         Удалить</button>
      </li>
    </ul>
    <div v-else-if="tasksCompleted.length">Все задачи выполнены!</div>
    <div v-else>Нет задач для выполнения. Добавьте первую.</div>
    <ul 
      class="todos__completed-list"
      v-if="tasksCompleted.length"
    >
      <li 
        class="todos__completed-item"
        v-for="taskCompleted in tasksCompleted"
        :key="taskCompleted.id"
      >
       <button type="submit" 
          class="todos__icon-mark"
          @click="revealTask(i)"
        
        >
          <svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="24" height="24" viewBox="0 0 1024 1024">
            <title/>
            <g id="check-img">
            </g>
            <path d="M698.24 416.96l-32.064-32.064c-5.92-5.92-15.488-5.92-21.376 0l-192.448 192.416-91.488-92.224c-5.952-5.92-15.488-5.92-21.44 0l-32.064 32.096c-5.92 5.92-5.92 15.488 0 21.376l134.176 135.136c5.92 5.888 15.488 5.888 21.376 0l235.328-235.328c5.952-5.92 5.952-15.488 0-21.408zM512 96c-229.76 0-416 186.24-416 416s186.24 416 416 416 416-186.24 416-416-186.24-416-416-416zM512 832c-176.704 0-320-143.296-320-320 0-176.736 143.296-320 320-320 176.736 0 320 143.264 320 320 0 176.704-143.264 320-320 320z"/>
          </svg>
        </button>
       <p 
        :class="{'active': taskCompleted.isCompleted}"
       >
        {{ taskCompleted.text }}
       </p> 
      </li>
    </ul>
    <div 
      class="calculatedTasks"
      v-if="tasks.length || tasksCompleted.length"
    >
      <p class="total">Задач в <span class="todos__progress">процессе</span>:  <span class="todos__progress">{{ tasks.length }}</span></p>
      <p class="total">Задач <span class="todos__complete">выполнено</span>: <span class="todos__complete">{{ tasksCompleted.length }}</span></p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'todos',
  data() {
    return {
      inputValue: '',
      tasks: [],
      tasksCompleted: [],
      id: 0,
    }
  },
  methods: {
    addTask() {
      if (this.inputValue != '') {

        this.tasks.push({
          id: this.id++,
          text: this.inputValue,
          isCompleted: false,
          });
        this.inputValue = '';
      }
    },
    removeTask(taskId) {
        this.tasks = this.tasks.filter(task => {
          return task.id !== taskId;
        })
    },
    clearInput() {
      this.inputValue = ''
    },
    completeTask(i) {
      let complete = this.tasks.splice(i, 1)
      this.tasksCompleted.push(...complete)
      this.tasksCompleted.forEach(taskCompleted => {
        return taskCompleted.isCompleted = true
      })
    },
    revealTask(i) {
      let reveal = this.tasksCompleted.splice(i, 1)
      this.tasks.push(...reveal)
       this.tasks.forEach(task => {
        return task.isCompleted = false
      })
    }
  }
}
</script>

<style lang="scss">

  .check-task {
      padding: 10px 15px;
      background-color: #fff;
      border: unset;
      box-shadow: inset 0 0 0 1px black;
  }
  .todos {

      .btn {
        background-color: transparent;
        color:$mainColor;
        padding: 5px 12px;
        
        box-shadow: inset 0 0 0 1px $mainColor;
        transition: all 0.3s ease 0s;

        &:hover {
          background-color: $mainColor;
          color: #fff;
          transition: all 0.3s ease 0s;
        }
      }
      &__icon-mark {
        cursor: pointer;
        margin: 0px 10px 0px 0px;

        display: flex;
        align-items: center;
        justify-content: center;

        svg {
          fill: #ccc;
          transition: all 0.4s ease 0s;

          &:hover {
            transition: all 0.4s ease 0s;
            fill: #03bb03;
          }
        }
      }
      &__icon-mark.active, &__completed-item {
        svg {
          fill: #03bb03;
        }
      }
    
      &__list, &__completed-list {
        list-style: none;
        padding: 0px 0px 30px 0px;
        border-bottom: 1px solid $mainColor; 
      }
      &__completed-list {
        padding: 30px 0px 30px 0px;
      }
      &__item, &__completed-item {
        position: relative;
        display: flex;
        align-items: center;
        text-align: left;
        text-transform: capitalize;
        color: $mainColor;

        p {
          font-size: 1rem;
        }

        p.active {
          &:after {
            content: 'Done!';
            color: #03bb03;
            margin: 0px 0px 0px 10px;
          }

        }
        
        .item-btn {
          position: absolute;
          top: 0;
          right: 0;
          
          &:hover {
          background-color: red;
          color: #fff;
          transition: all 0.3s ease 0s;
        }
        }
      }
      &__item:not(:last-child), &__completed-item:not(:last-child) {
        margin: 0px 0px 5px 0px;
      }
        &__input-wrapper {
        position: relative;
        max-width: 450px;
        width: 100%;
        height: 26px;

        display: flex;
        align-items: center;

        & input{
          position: absolute;
          top: 0;
          left: 0;
          width: 100%;
          height: 100%;
          color: $mainColor;
          box-shadow: inset 0 0 0 1px $mainColor;

          padding: 12px;
            &::placeholder {
              color: $mainColor;
            }
        } 
        .remove-btn {
          width: 30px;
          height: 26px;
          position: absolute;
          right: 0;
          top: 0;
          background-color: transparent;

          &:after, &:before {
            content: '';
            position: absolute;
            width: 10px;
            height: 1px;
            background-color: $mainColor;
            top: 50%;
            left: 50%;
            transform: translate(-50% , -50%);
          }
          &:after {
            transform: translate(-50% , -50%) rotate(45deg);
          }
          &:before {
            transform: translate(-50% , -50%) rotate(-45deg);
          }
        }
      }
        &__input-box {
        display: flex;
        column-gap: 20px;
        padding: 0px 0px 30px 0px;
      }
        &__line {
        width: 100%;
        height: 1px;
        background-color: $mainColor;
        margin: 0px 0px 30px 0px;
      }
      &__progress {
        color: red;
        text-transform: uppercase;
      }
       &__complete {
        color: #03bb03;
        text-transform: uppercase;
      }
  }
  .calculatedTasks {
    display: flex;
    align-items: center;
    justify-content: flex-start;
    column-gap: 50px;
    color: $mainColor;
    font-size: 1rem;
    padding: 30px 0;
  }  

</style>
