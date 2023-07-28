<script setup>
  import { ref, defineEmits, watch } from "vue";

  const emit = defineEmits(['RegionSelect'], ['SearchPokemon'])
  let dropdownOpen = ref(false);
  let region = ref('kanto');
  let search = ref('')
  
  function dropdownClick(){dropdownOpen.value = !dropdownOpen.value};

  watch(region, () =>{
    emit('RegionSelect', region.value)
  })

  function SearchPokemon(){
    emit('SearchPokemon', search.value.value);
  }


</script>

<template>
  <header>
    <nav class="navbar">
      <div class="logoContent">
        <div class="logo">
          <img src="../../public/img/pokemon.svg" alt="">
          <div class="logoName">Pokedex</div>
        </div>
        <i class="bx bx-menu" id="btn"></i>
      </div>
      <ul>
        <li class="itensHeader">
          <i class="bx bx-search"></i>
          <input type="text" placeholder="search" ref="search" @keyup.enter="SearchPokemon()"/>
        </li>
        <li :class="{dropDownActive: dropdownOpen, dropDownInative: !dropdownOpen}">
          <div href="#" id="dropdownMenu" @click="dropdownClick()" >
            <i class="bx bxs-user"></i>
            <span>Region</span>
            <i class='bx bxs-down-arrow'></i>
          </div>
          <ul class="submenu" v-if="dropdownOpen">
            <li><a @click.prevent="region = 'kanto', search.value = ''">Kanto</a></li>
            <li><a @click.prevent="region = 'johto', search.value = ''">Johto</a></li>
            <li><a @click.prevent="region = 'hoenn', search.value = ''">Hoenn</a></li>
            <li><a @click.prevent="region = 'sinnoh', search.value = ''">Sinnoh</a></li>
            <li><a @click.prevent="region = 'unova', search.value = ''">Unova</a></li>
            <li><a @click.prevent="region = 'kalos', search.value = ''">Kalos</a></li>
            <li><a @click.prevent="region = 'alola', search.value = ''">Alola</a></li>
            <li><a @click.prevent="region = 'galar', search.value = ''">Galar</a></li>
            <li><a @click.prevent="region = 'paldea', search.value = ''">Paldea</a></li>
          </ul>
        </li>
        <li class="itensHeader">
          <a href="#" class="buttonHeader">
            <i class="bx bxs-user"></i>
            <span>User</span>
          </a>
        </li>
        <li class="itensHeader">
          <a href="#" class="buttonHeader">
            <i class="bx bx-pie-chart-alt-2"></i>
            <span>Analytics</span>
          </a>
        </li>
        <li class="itensHeader">
          <a href="#" class="buttonHeader">
            <i class="bx bx-plus-circle"></i>
            <span>Create Team</span>
          </a>
        </li>
      </ul>
      <div class="profileContent">
        <div class="profile">
          <div class="profileDetails">
            <img src="http://placekitten.com/150" alt="" />
            <div class="name">Catmon</div>
            <i class="bx bx-log-out" id="logOut"></i>
          </div>
        </div>
      </div>
    </nav>
  </header>
</template>

<style>
@import url("https://fonts.googleapis.com/css2?family=Caveat:wght@700&family=Montserrat&family=Nunito+Sans:wght@300;700&family=Poppins:wght@500&display=swap");

* {
  font-family: "Poppins";
}
.navbar {
  position: fixed;
  top: 0;
  left: 0;
  width: 240px;
  height: 100%;
  background-color: var(--headerColor);
  font-size: 22px;
  font-weight: 500px;
  padding: 6px 14px;
}

.logoContent {
  color: #fff;
  display: flex;
  height: 50px;
  width: 100%;
  align-items: center;
}
.logo{
  display: flex;
}

.logo img{
  width: 28px;
  margin-right: 5px;
}

.logoName {
  font-size: 20px;
  font-weight: 400;
}

.logoContent #btn {
  position: absolute;
  color: #fff;
  left: 90%;
  top: 6px;
  font-size: 20px;
  height: 50px;
  width: 50px;
  text-align: center;
  line-height: 50px;
  transform: translateX(-50%);
}
.navbar ul{
  margin-top: 20px;
}

.navbar .submenu{
  margin-top: 0px;
}

.navbar ul .itensHeader{
  position: relative;
  height: 50px;
  width: 100%;
  margin: 0px 5px;
  list-style: none;
  line-height: 50px;
  margin: 0 auto;
}

.navbar ul li input{
  position: absolute;
  height: 100%;
  width: 100%;
  left: 0;
  top: 0;
  border-radius: 12px;
  outline: none;
  border: none;
  background:#1d1b31;
  padding-left: 50px;
  font-size: 18px;
  color: #fff;
  margin-bottom: 20px;
}

.navbar ul li .bx-search{
  position: absolute;
  z-index: 99;
  color: #fff;
  font-size: 22px;
}

.navbar ul .itensHeader a, #dropdownMenu{
  border-radius: 12px;
  color: #fff;
  display: flex;
  align-items: center;
  text-decoration: none;
  transition: all 0.4 ease;
}

.navbar ul li a:hover{
  color: #11101d;
  background-color: #fff;
}
.dropDownInative #dropdownMenu:hover{
  color: #11101d;
  background-color: #fff;
}

.navbar ul li i{
  height: 50px;
  min-width: 50px;
  border-radius: 12px;
  line-height: 50px;
  text-align: center;
}
.navbar ul li .bxs-down-arrow{
  position: absolute;
  font-size: 12px;
  left: 75%;
}

.dropDownInative{
  cursor: pointer;
  width: 100%;
  list-style: none;
  line-height: 40px; 
  margin-top: 10px;
}

.dropDownInative #dropdownMenu .bxs-down-arrow{
  transform: rotate(-90deg);
}

.dropDownActive{
  cursor: pointer;
  background-color: #1d1b31;
  border-radius: 12px;
  margin: 0px 5px;
  list-style: none;
  line-height: 40px;
  width: 100%;
  margin: 10px 0px;
}

.dropDownActive .submenu li:hover{
  background-color: var(--headerColor);
}

.dropDownActive li{
  list-style: none;
  margin-left: 40px;
  font-size: 18px;
}

.dropDownActive li a{
  border-radius: 3px;
  color: #fff;
  display: flex;
  align-items: center;
  text-decoration: none;
  transition: all 0.4 ease;
  padding-left: 10px;
}
.navbar .profileContent{
  position: absolute;
  color: #fff;
  bottom: 0;
  left: 0;
  width:100%;
}
.navbar .profileContent .profile{
  position: relative;
  padding: 10px 6px;
  height: 60px;
  background: #1d1b31 ;
}
.navbar .profileContent .profile .profileDetails{
  display: flex;
  align-items: center;
}
.profile .profileDetails img{
  height: 45px;
  width: 45px;
  object-fit: cover;
  border-radius: 12px;
}
.profile .profileDetails .name{
  font-size: 15px;
  font-weight: 400;
  margin-left: 5px;
}

.profile #logOut{
  position: absolute;
  left: 88%;
  bottom: 5px;
  transform: translateX(-50%);
  min-width: 50px;
  line-height: 50px;
  font-size: 20px;
  border-radius: 12px;
}
</style>
