<script setup>
  import { ref, defineEmits, watch } from "vue";

  const emit = defineEmits(['RegionSelect'], ['SearchPokemon'])
  let dropdownOpen = ref(false);
  let region = ref('kanto');
  let search = ref('');
  let menu = ref(false)
  
  function dropdownClick(){
    dropdownOpen.value = !dropdownOpen.value;
    
  };

  function menuClick(){
    menu.value = !menu.value
  }

  watch(region, () =>{
    emit('RegionSelect', region.value)
  })

  function SearchPokemon(){
    emit('SearchPokemon', search.value.value);
  }


</script>

<template>
  <header>
    <nav class="navbar" :class="{navbarActive: menu,  navbarInative: !menu}">
      <div  class="logoContent">
        <div class="logo">
          <img @click="menu=true" class="logoImg" src="../../public/img/pokemon.svg" alt="">
          <div class="logoName"><span>Pokedex</span></div>
        </div>
        <i @click="menu = false" class="bx bx-x closeButton"></i>
      </div>
      <ul>
        <li class="itensHeader">
          <i @click="menu = true"  class="bx bx-search"></i>
          <input type="text" placeholder="search" ref="search" @keyup.enter="SearchPokemon()"/>
        </li>
        <li :class="{dropDownActive: dropdownOpen, dropDownInative: !dropdownOpen}">
          <div class="regionButton" href="#" id="dropdownMenu" @click="dropdownClick()" >
            <div>
              <i class="bx bxs-user"></i>
              <span>Region</span>
            </div>
            <i class='bx bxs-down-arrow'></i>
          </div>
          <ul class="submenu" v-if="dropdownOpen">
            <li><a @click.prevent="region = 'kanto', search.value = '', dropdownOpen = false, menu = false">Kanto</a></li>
            <li><a @click.prevent="region = 'johto', search.value = '', dropdownOpen = false, menu = false">Johto</a></li>
            <li><a @click.prevent="region = 'hoenn', search.value = '', dropdownOpen = false, menu = false">Hoenn</a></li>
            <li><a @click.prevent="region = 'sinnoh', search.value = '', dropdownOpen = false, menu = false">Sinnoh</a></li>
            <li><a @click.prevent="region = 'unova', search.value = '', dropdownOpen = false, menu = false">Unova</a></li>
            <li><a @click.prevent="region = 'kalos', search.value = '', dropdownOpen = false, menu = false">Kalos</a></li>
            <li><a @click.prevent="region = 'alola', search.value = '', dropdownOpen = false, menu = false">Alola</a></li>
            <li><a @click.prevent="region = 'galar', search.value = '', dropdownOpen = false, menu = false">Galar</a></li>
            <li><a @click.prevent="region = 'paldea', search.value = '', dropdownOpen = false, menu = false">Paldea</a></li>
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
.navbarActive {
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
.navbarInative {
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
  flex-direction: row;
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
.bx-menu{
  display: none;
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
  background:var(--black);
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
.regionButton{
  display: flex;
  justify-content: space-between;
}
.navbar ul li .bxs-down-arrow{
  font-size: 12px;
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
  background-color: var(--black);
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
  background: var(--black) ;
}
.navbar .profileContent .profile .profileDetails{
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 0px 5px;
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

.closeButton{
  display: none;
}


@media(max-width: 600px) {
  .navbar {
    width: 70px;
  }
}

@media(max-width: 450px) {
  .logoName{
    
  }
  .bx-menu{
    display: block;
  }

  .navbarActive{
    z-index: 10;
    width: 100%; 
  }

  .navbarInative{
    position: fixed;
    z-index: 2;
    margin: 10px;
    width: 70px;
    height: 70px;
    border-radius: 20px;
    align-items: center;
    top:88%;
    box-shadow: 2px 2px 2px 1px rgba(0, 0, 0, 0.2);
  }
  .navbarInative > .logo{
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
  }

  .navbarActive .logoContent{
    display: flex;
    justify-content: space-between;
    font-size: 30px;
  }

  .navbarActive .logoContent .logo{
    display: flex;
    flex-direction: row;
    justify-content: space-around;
  }
  .navbarActive ul li .bxs-down-arrow{
    left: 80%;
  }
  .navbarActive .logoContent .closeButton{
    display: block;
  }

  .navbarInative .logoContent .closeButton{
    display: none;
  }

  .navbarInative .logoContent{
    justify-content: center;
  }

  .navbarInative .logoContent .logo{
    display: flex;
    justify-content: center;
    align-items: center;
    align-content: center;
  }
  .navbarInative .logoContent .logo .logoImg{
    width: 50px;
    margin: 5px;
    margin-top: 10px;
  }

  .navbarInative .logoContent .closeButton{
    display: none;
  }

  .navbarInative span{
    display: none;
  }


  .navbarInative > ul{
    display: none;
  }

  .navbarInative > .profileContent {
    display: none;
  }
}
</style>
