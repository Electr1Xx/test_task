<template>
  <div id="app">
<div class="tripl" v-for="(triple,index) in tripls" >
    <div class="subject">
    <input class="name" type="text"  v-model="triple.subject.name" >
      <input :id="'check'+index" type="checkbox" v-model="triple.subject.active" >
      <label :for="'check'+index"></label>
    <button class="addThisTripl" v-on:click="addNewTriplWithName(triple.subject.name,'subject')">+</button>
    </div>
    <div class="predicat">
      <input class="name" type="text"  v-model="triple.predicat.name">
      <input :id="'checkPredicat'+index" type="checkbox" v-model="triple.predicat.active" >
      <label :for="'checkPredicat'+index"></label>
      <button class="addThisTripl" v-on:click="addNewTriplWithName(triple.predicat.name,'predicat')">+</button>
    </div>
    <div class="object">
      <input class="name" type="text"  v-model="triple.object.name">
      <input :id="'checkObject'+index"  type="checkbox" v-model="triple.object.active" >
      <label :for="'checkObject'+index"></label>
      <button class="addThisTripl" v-on:click="addNewTriplWithName(triple.object.name,'object')">+</button>
    </div>
  <button class="delete" v-on:click="deleteGroup(index)">X</button>
</div>
    <div class="footerTripl">
    <button class="addTripl" v-on:click="addTripl()">+</button>
    <textarea v-model="buildString"  name="" id="" cols="30" rows="10">
    </textarea>
    <button class="build" v-on:click="buildResponse">Build</button>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data () {
    return {
      buildString: '',
      checkedTripl: [],
      tripls: []
    }
  },
  methods: {
    addTripl: function () {
      this.tripls.push({
        subject: {
          name: '',
          active: false
        },
        predicat: {
          name: '',
          active: false
        },
        object: {
          name: '',
          active: false,
          new: true
        }
      })
    },
    addNewTriplWithName: function (value, name) {
      let newTripl = {
        subject: {
          name: '',
          active: false
        },
        predicat: {
          name: '',
          active: false
        },
        object: {
          name: '',
          active: false,
          new: true
        }
      }
      if (name === 'subject') {
        newTripl.subject.name = value
      } else if (name === 'predicat') {
        newTripl.predicat.name = value
      } else {
        newTripl.object.name = value
      }
      this.tripls.push(newTripl)
    },
    deleteGroup: function (index) {
      this.tripls.splice(index, 1)
    },
    buildResponse: function () {
      let activeItem = ''
      let items = ''
      this.tripls.forEach(function (item) {
        for (var prop in item) {
          if (item[prop].new === true)
          {
            items = items + item[prop].name + '\n'
          } else {
            items = items + item[prop].name + ' '
          }
          if (item[prop].active === true) {
            activeItem = activeItem + item[prop].name + ' '
          }
        }
      })
      this.buildString = 'SELECT ' + activeItem + ' WHERE { \n ' + items + '}'
    }
  }
}
</script>

<style>
  #app{
    width: 50%;
    margin: 0 auto;
  }
.tripl{
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
}
  .footerTripl{
    display: flex;
    flex-direction: column;
    margin-top: 20px;
  }
  button.addTripl{
    width: 30px;
    height: 30px;
    padding: 0;
    border-radius: 100%;
    margin-bottom: 20px;
  }
  button.build{
    margin-top: 20px;
    width: 70px;
    margin-left: auto
  }
  .subject, .predicat, .object {
    border: 1px solid;
    padding: 3px;
  }
  .name{
    border: none;
  }
  .addThisTripl{
    border-radius: 100%;
  }
  .delete {
    border-radius: 100%;
    width: 30px;
    height: 30px;
  }
  label {
    cursor: pointer;
    position: relative;
    padding-left: 25px;
    font-size: 13px;
  }
  label:before {
    content: "";
    display: inline-block;

    width: 16px;
    height: 16px;

    margin-right: 10px;
    position: absolute;
    left: 0;
    bottom: 1px;
    background-color: #aaa;
    box-shadow: inset 0px 2px 3px 0px rgba(0, 0, 0, .3), 0px 1px 0px 0px rgba(255, 255, 255, .8);
  }
  input[type=checkbox]{
    display: none;
  }
  input[type=checkbox]:checked + label:before {
    content: "\2022";
    color: #f3f3f3;
    font-size: 30px;
    text-align: center;
    line-height: 17px;
  }
  label:before {
    border-radius: 8px;
  }

</style>
