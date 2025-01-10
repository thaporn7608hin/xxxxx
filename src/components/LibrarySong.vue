<template>
  <div :class="['library', { 'library-open': isSidebarOpen }]">
     <div class="dropdown-center"  style="position: absolute;top: 2.5%; left: 3%;">
          <button
          id="hover-btn"
            class="btn dropdown-toggle px-4 text-white"
           
            type="button"
            data-bs-toggle="dropdown"
            aria-expanded="false"
          >
            Style Music
          </button>
          <ul class="dropdown-menu">
            <li style="margin-top: 0;"><button class="dropdown-item" @click="()=>filterSong('j-pop')" >J-pop</button></li>
            <li><button class="dropdown-item" @click="()=>filterSong('ai')" >Ai Music</button></li>
            <li><button class="dropdown-item" @click="()=>filterSong('sad')" >Sad</button></li>
            <li><button class="dropdown-item" @click="filterSong()">All</button></li>
          </ul>
        </div>
    <button class="toggle-btn" @click="toggleSidebar">
      <svg xmlns="http://www.w3.org/2000/svg" width="60" height="40" fill="currentColor" class="bi bi-list" viewBox="0 0 16 16">
        <path fill-rule="evenodd" d="M2.5 12a.5.5 0 0 1 .5-.5h10a.5.5 0 0 1 0 1H3a.5.5 0 0 1-.5-.5m0-4a.5.5 0 0 1 .5-.5h10a.5.5 0 0 1 0 1H3a.5.5 0 0 1-.5-.5m0-4a.5.5 0 0 1 .5-.5h10a.5.5 0 0 1 0 1H3a.5.5 0 0 1-.5-.5"/>
      </svg>
    </button>
    <ul style="overflow-y: scroll">
      <li
        v-for="(song, index) in songs"
        :key="index"
        @click="$emit('select-song', song.id)"
      >
        <div id="box" :style="song.active ? {backgroundColor: '#d1d5db'} : {}" class="w-100">
          <div class="d-flex align-items-center">
            <img :src="song.img" alt="" style="width: 70px; height: 70px" class="me-2" />
            <p>{{ song.title }}</p>
          </div>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    songs: {
      type: Array,
      required: true,
    },
    filterSong:{
      type:Function,
      required:true
    }
  },
  data() {
    return {
      isSidebarOpen: false,
    };
  },
  methods: {
    toggleSidebar() {
      this.isSidebarOpen = !this.isSidebarOpen;
    },
  },
};
</script>

<style>
h2 {
  margin-bottom: 3rem;
}
.toggle-btn {
  position: absolute;
  top: 2.5%;
  margin-left: 12px;
  width: 60px;
  height: 40px;
  border: none;
  border-radius: 6px;
  background-color: #d1d5db;
}
.toggle-btn:active {
  background-color: black;
  color: white;
}
.dropdown-menu.show{
  max-height: 15rem;
}
.library {
  position: fixed;
  left: 0;
  top: 80px;
  background-color: #f0f0f0;
  height: 88.5vh;
  width: 20rem;
  transform: translateX(-100%);
  transition: 0.3s ease-out;
  z-index: 10;
}

.library-open{
  transition: 0.3s ease-in;
  transform: translateX(0)
}
.library .toggle-btn{
  transition: 0.3s ease-out;
  left: 20rem;
}
.library-open .toggle-btn{
   transition: 0.3s ease-in;
   
  left: 70%;
}
.library ul {
  list-style-type: none;
  padding: 0;
  height: 38rem;
  overflow-y: scroll;
}
.library li {
  padding: 10px;
  cursor: pointer;
}
.library li:nth-of-type(1) {
  margin-top: 4rem;
}
.library li div {
}
#box:hover {
  background-color: #d1d5db;
}
</style>
