<template lang="html">
    <div class="container">
      <div class="grid">
        <div class="tab-header">
          <div class="large-border">
            <div class="small-border">
              <ToolBar ref="activeTab"></ToolBar>
            </div>
          </div>
        </div>
        <div class="repeat-border">
        </div>
        <div class="head1">
          <h1 id="title" style="text-align:left;float:left;" class="animate fadeInLeft">
            <vue-typer
              :text='name'
              :repeat='0'
              :typeDelay='30'
              :pre-type-delay='500'
              caret-animation='solid'></vue-typer>
            </h1>
        </div>
        <div class="head2 time-container">
          <!-- <h1 class="clock-container animated fadeIn"> -->

        </div>
        <div class="col1">
          <div class="selection animated fadeInLeft bookmark-wrapper">
            <div style="overflow-x: hidden;
                        overflow-y: auto;
                        height: 500px;
                        width: 95%;">
              <div class="large-border">
                <div class="small-border">
                  <ListBookmarks></ListBookmarks>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="col2">
          <div  v-if="renderNotePad">
            <NotePad class="animated fadeIn"></NotePad>
          </div>
        </div>
        <div v-if="renderMostVisitedSites" class="col-only-2">
          <TopSites  class="animated fadeIn"></TopSites>
        </div>
          <div v-if="renderTodo" class="col2">
            <Todo class="animated fadeIn"></Todo>
          </div>
        <Bottom>
        </Bottom>
      </div>
      <div class="modals">
      </div>
    </div>
</template>

<script>
import NotePad from './notepad.vue';
import ListBookmarks from './list-bookmarks.vue';
import TopSites from './top-sites.vue';
import Modal from './modal.vue';
import ToolBar from './tool-bar.vue';
import Bottom from './footer.vue';
import Todo from './todo.vue';
import {
  VueTyper
} from 'vue-typer';
import VueLocalStorage from 'vue-localstorage';
import Vue from 'vue';
import $ from 'jquery';

Vue.use(VueLocalStorage)

function getTime() {
  var date = new Date;
  var hours = date.getHours();
  var minutes = date.getMinutes();
  // var seconds = date.getSeconds();
  var ampm = hours >= 12 ? 'pm' : 'am';
  hours = hours % 12;
  hours = hours ? hours : 12; // the hour '0' should be '12'
  minutes = minutes < 10 ? '0'+minutes : minutes;
  // seconds = seconds < 10 ? '0'+seconds : seconds;
  var strTime = hours + ':' + minutes + ' ' + ampm;

  return strTime;
}

function randomCarrot(component, text) {
  var arr = text.split('');
  var run = true;
  // component.animation = window.requestAnimationFrame(function(frame) {
  //   var char = arr[Math.floor(Math.random()*arr.length)];
  //   document.querySelector('.custom.caret').innerHTML = char;
  // })
  document.querySelector('.custom.caret').classList.add('display-none');
  setTimeout(function() {
    document.querySelector('.custom.caret').classList.remove('display-none');
  }, 500)
  window.setInterval(function() {
    var char = arr[Math.floor(Math.random() * arr.length)];
    document.querySelector('.custom.caret').innerHTML = char;
  }, 50)
}

export default {
  data() {
    return {
      name: 'New Tab',
      square: '■',
      textComplete: false,
      time: null,
      activeTab: Vue.localStorage.get('activeTab'),
      renderNotePad: false,
      renderMostVisitedSites: false,
      renderTodo: false
    }
  },
  created() {
    this.$nextTick(function() {
      $('.time-container').append('<h1 class="clock-container time animated fadeIn"></h1>')
      $('.time').text(getTime());
      randomCarrot(this, this.name);
      setInterval(function () {
        $('.time').text(getTime());
      }, 1000)
    })
  },
  mounted() {
    if (this.activeTab) {
      this.setAppActiveTab(this.activeTab)
    }
  },
  methods: {
    setAppActiveTab: function(newActiveTab) {
      var activeTab = newActiveTab;

      if (activeTab === 'Note Pad') {
        this.renderNotePad = true
      } else {
        this.renderNotePad = false
      }

      if (activeTab === 'Top Visited Sites') {
        this.renderMostVisitedSites = true
      } else {
        this.renderMostVisitedSites = false
      }
      if (activeTab === 'Todo List') {
        this.renderTodo = true
      } else {
        this.renderTodo = false
      }
      return Vue.localStorage.set('activeTab', activeTab);
    }
  },
  components: {
    ListBookmarks: ListBookmarks,
    NotePad: NotePad,
    TopSites: TopSites,
    // Clock: Clock,
    Modal: Modal,
    ToolBar: ToolBar,
    Bottom: Bottom,
    Todo: Todo,
    VueTyper
  }
}
</script>

<style lang="css">
  figure {
    margin-left: 2em;
    height: 600px;
  }
  h1 {
    -webkit-margin-before: 0;
  }
  .clock-container {
    text-align: right;
  }
  .clock {
    font-family: helvetica, sans-serif;
    font-weight: 200;
    text-transform: uppercase;
    letter-spacing: 0.5rem;
    text-shadow: 0.3rem 0.3rem 0 #bab5a1;
    display: block;
    font-size: 4em;
    -webkit-margin-before: 0;
    -webkit-margin-after: 0.67em;
    -webkit-margin-start: 0px;
    -webkit-margin-end: 0px;
  }
  .extend-panel {
    background-color: #dcd8c0;
    padding: 1em;
  }
  .list-panel .top-sites-panel {
    padding: 0.5em 0;
  }
  .list-panel .custom-button {
    width: 95% !important;
  }
  .list-panel button {
    margin-left: 0;  }
  .list-panel a {
    margin-left: 0;
    padding-left: 0;
  }
  .list-panel button:hover {
    background-color: #454138;
  }
  /*.list-panel {
    background-color: rgb(220, 216, 192);
    padding: 0.5em;
    margin-left: 3em;
  }
  .list-panel .top-sites-panel {
    padding: 0.5em 0;
  }
  .list-panel .custom-button {
    width: 100%;
  }
  .list-panel button {
    margin-left: 0;  }
  .list-panel a {
    margin-left: 0;
    padding-left: 0;
  }
  .list-panel button:hover {
    background-color: #454138;
  }*/
</style>
