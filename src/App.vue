<template>
  <div id="app">
     <p v-if="errors.length">
    <b>Please correct the following error(s):</b>
    <ul>
      <li v-for="(error,index) in errors" :key="index">{{ error }}</li>
    </ul>
  </p>
    <div class="add-ticket-wrapper">
      
      <form @submit.prevent="onSubmit">
        <div>
          <label for="">Type </label>
          <input type="text" v-model="form.type">
        </div>
        <div>
          <label for="">Description </label>
          <input type="text" v-model="form.desc">
        </div>
        <div>
          <label for="">Fine </label>
          <input type="number" v-model="form.fine">
        </div>
        <div>
          <label for="">isPaid </label>
          <label class="switch">
          <input type="checkbox" v-model="form.isPaid">
          <span class="slider round"></span>
        </label>
        </div>
        <button type="submit">{{ isEditing ? "update":"add"}}</button>
         <button v-if="!isEditing" @click="cancelAdd">cancel</button>
        <button v-if="isEditing" @click="cancelUpdate">cancel</button>
      </form>

    </div>


    <div>
        <table class="table table-hover" v-if="sourceArray.length">
          <thead>
          <tr>
            <th>type</th>
            <th>desc</th>
            <th>fine</th>
            <th>isPaid</th>
          </tr>
          </thead>
          <tr v-for="(item, index) in sourceArray" :key="index" :class="{ editing:checkEditing(index)}">
            <th>{{item.type}}</th>
            <th>{{item.desc}}</th>
            <th>{{item.fine}}</th>
            <th>{{item.isPaid}}</th>
            <th><button @click="updateRow(index)">edit</button><button @click="deleteRow(index)">delete</button></th>
            
          </tr>
        </table>
    </div>
  </div>


</template>

<script>

export default {
  name: 'App',
  data: function() {
    return {
      isEditing:false,
      errors : [],
      editIndex:-1,
      form: { type:'',desc:'',fine:0 ,isPaid:false},
      sourceArray:[
      ]
    }
  },
  methods:{
     checkForm: function (e) {
      this.errors = [];

      if (!this.form.type) {
        this.errors.push("type required.");
      }
      if (!this.form.desc) {
        this.errors.push('desc required.');
      }
      if (!this.errors.length) {
        return true;
      }

      e.preventDefault();
    },
    checkEditing(index){
      return (this.editIndex === index)
    },
    cancelAdd(){
      this.form = { type:'',desc:'',fine:0 ,isPaid:false}

    },
    cancelUpdate(){

      this.editIndex = -1
      this.isEditing = false
      this.form = { type:'',desc:'',fine:0 ,isPaid:false}
    },
    paid(index) {
      // this.sourceArray.remove
      this.sourceArray[index].isPaid = !this.sourceArray[index].isPaid
      
    },
    updateRow(index) {
      // this.sourceArray.remove
      this.editIndex = index
      this.form = {...this.sourceArray[index]}
      this.isEditing = true
      
    },
    deleteRow(index) {
      // this.sourceArray.remove
      this.sourceArray.splice(index,1)
      // console.log(index)
    },
    onSubmit() {
      
      if (this.editIndex > -1) {
        this.sourceArray[this.editIndex] = {...this.form}
      }
      else {
        this.checkForm()
        this.sourceArray = [...this.sourceArray, {...this.form} ]
      }
      this.editIndex = -1
      this.isEditing = false
      this.form = { type:'',desc:'',fine:0 ,isPaid:false}
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#table {
  border-width: 2px;
  border-color: aquamarine;
}

.editing {
  background:green;
}
.switch {
  position: relative;
  display: inline-block;
  width: 40px;
  height: 24px;
}

.switch input { 
  opacity: 0;
  width: 0;
  height: 0;
}

.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  -webkit-transition: .4s;
  transition: .4s;
}

.slider:before {
  position: absolute;
  content: "";
  height: 16px;
  width: 16px;
  left: 4px;
  bottom: 4px;
  background-color: white;
  -webkit-transition: .4s;
  transition: .4s;
}

input:checked + .slider {
  background-color: #2196F3;
}

input:focus + .slider {
  box-shadow: 0 0 1px #2196F3;
}

input:checked + .slider:before {
  -webkit-transform: translateX(26px);
  -ms-transform: translateX(26px);
  transform: translateX(26px);
}

/* Rounded sliders */
.slider.round {
  border-radius: 34px;
}

.slider.round:before {
  border-radius: 50%;
}
</style>