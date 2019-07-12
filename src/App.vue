<template>
  <div class="container">
    <div class="row">
      <div class="col-md-12">
        <h1>Animations</h1>
        <hr>
        <h3>Css Animations</h3>
        <select v-model="alertAnimation" class="form-control">
          <option value="fade">Fade</option>
          <option value="slide">Slide</option>
        </select>
        <br>
        <button class="btn btn-primary" @click="show = !show">Show Alert</button>
        <br><br>
        <transition :name="alertAnimation">
        <div class="alert alert-info" v-if="show">Some Alert</div>
      </transition>
        <transition name="slide" type="animation">
          <div class="alert alert-info" v-if="show">Animated Alert</div>
        </transition>
        <transition appear enter-active-class="animated bounce" leave-active-class="animated shake">
          <div class="alert alert-info" v-if="show">Animated Alert</div>
        </transition>
        <transition :name="alertAnimation" mode="out-in">
          <div class="alert alert-warning" v-if="show" key="warning">Some Alert</div>
          <div class="alert alert-info" v-if="!show" key="info">Some Info</div>
        </transition>
        <hr>
        <h3>Js Animations</h3>
        <button class="btn btn-primary" @click="load =!load">Load / Remove Element</button>
        <br><br>
        <transition @before-enter="beforeEnter" @enter="enter" @before-leave="beforeLeave" @leave="leave">
          <div style="width: 300px; height: 100px; background-color: lightgreen" v-if="load"></div>
        </transition>
        <hr>
        <h3>Dynamic Component</h3>
        <button class="btn btn-primary" @click="selectedComponent == 'app-success' ? selectedComponent = 'app-danger' : selectedComponent = 'app-success'">Toggle Components</button>
        <transition name="fade" mode="out-in">
          <component :is="selectedComponent"></component>
        </transition>
        <hr>
        <div class="form-group">
          <label>Add a language</label>
          <input type="text" v-model="addLang">
        </div>
        <button class="btn btn-primary" @click="addItem">Add Item</button>
        <ul class="list-group">
        <transition-group name="slide">
            <li class="list-group-item" v-for="(item,index) in langs" :key="item" @click="removeItem(index)">{{ item }}</li>
        </transition-group>
        </ul>


      </div>
    </div>

  </div>
</template>

<script>
import DangerAlert from './components/DangerAlert';
import SuccessAlert from './components/SuccessAlert';

export default {
  name: 'app',
  data(){
    return {
        load:true,
        show: false,
        alertAnimation:'fade',
        elWidth: 100,
        selectedComponent: 'app-success',
        langs:['java','python','c','c++'],
        addLang: ''
    }
  },
  methods: {
    addItem(){
      this.langs.push(this.addLang);
      this.addLang = '';
    },
    removeItem(index){
      this.langs.splice(index,1);
    },
    // js transition / animation
    beforeEnter(el){
      this.elWidth = 100;
      el.style.width  = this.elWidth + 'px';
    },

    enter(el,done){
      let round = 1;
      const interval = setInterval(()=> {
        el.style.width = (this.elWidth + round * 10) + 'px';
        round ++;
        if(round > 20){
          clearInterval(interval);
          done();
        }
      },20);

    },
    leave(el,done){
      let round = 1;
      const interval = setInterval(()=> {
        el.style.width = (this.elWidth - round * 10) + 'px';
        round ++;
        if(round > 20){
          clearInterval(interval);
          done();
        }
      },20);
    },
    beforeLeave(el){

      this.elWidth = 300;
      el.style.width = this.elWidth + 'px';
    }
  },
  components: {
   'app-danger': DangerAlert,
    'app-success':SuccessAlert
  }
}
</script>

<style>
  /* Beginning (attached for 1 frame at the beginning) Init State*/
  .fade-enter{
    opacity: 0;
  }
  /*   */
  .fade-enter-active {
      transition: opacity 1s;
  }

  .fade-leave {
    opacity: 1;
  }

  .fade-leave-active {
    transition: opacity 1s;
    opacity: 0;
  }

  .slide-enter{
    /*animation: slide-in 1s ease-out forwards;*/

  }

  .slide-enter-active{
      animation: slide-in 1s ease-out forwards;
    transition: opacity .5s;
  }
  .slide-leave{

  }
  .slide-leave-active{
    animation: slide-out 1s ease-out forwards;
    transition: opacity 1s;
    opacity: 0;
    //position: absolute;
  }

  //transition group
  .slide-move{
    transition: transform 1s;
  }

  @keyframes slide-in {
    from {
      transform: translateY(20px);
    }
    to {
      transform: translateY(0);
    }
  }

  @keyframes slide-out {
    from {
      transform: translateY(0);
    }
    to {
      transform: translateY(20px);
    }
  }
</style>
