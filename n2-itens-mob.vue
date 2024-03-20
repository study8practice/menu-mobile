<template>
    <nav class="n2-nav">
      <ul class="n3-lista">
        <li class="n3-itens" v-for="(n1, i) in menu" :key="i">
          <div class="n3-itens-wrapper" v-if="n1Index == i">
              <span class="n3-itens-label" @click="backMenu">{{n1.label}}</span>
            <ul class="n3-listaSub" v-if="n1.sub">
              <li v-for="(n2, idn2) in n1.sub" :key="idn2" class="n3-itensSub"
              @click="n2.sub ? openMenuMobN2() : false">{{n2.label}} 
                <svg class="iconOpenN3" v-if="n2.sub" data-v-2f9415a8="" width="12" height="12" viewBox="0 0 18 10" fill="none" xmlns="http://www.w3.org/2000/svg"><path data-v-2f9415a8="" d="M17 1L9.70711 8.29289C9.31658 8.68342 8.68342 8.68342 8.29289 8.29289L1 0.999999" stroke="#6B6B6B" stroke-linecap="round"></path></svg>            
                <ul class="n3-listaSubSub">
                  <li v-for="(n3, i) in n2.sub" :key="i" class="n3-itensSubSub">{{n3.label}}</li>
                </ul>
              </li>
            </ul>
          </div>
        </li>
      </ul>
    </nav>
</template>
<script>
import menujson from '../../api/saraiva.json';

export default {
  name: 'n2-itens-mob',
  props: ['n1Index', 'ativo'],
  data() {
    return {
      closeMenu: this.ativo
    }
  },
  computed: {
    menu(){
      return menujson.menu;
    },
  },
  methods: {
    backMenu(){
      this.closeMenu = false;
      this.$emit("close-menu", this.closeMenu);
    },
    openMenuMobN2() {
      const el = event.target;
      const elChil = event.target.children;

      if(el.classList.contains('n3-itensSub')){
        elChil[0].classList.toggle('iconAtivo')
        elChil[1].classList.toggle('active3')
        elChil[1].style.maxHeight = elChil[1].scrollHeight + "px";
      } 
      
      if(el.classList.contains('n3-itensSub') && !elChil[1].classList.contains('active3')){
        elChil[1].style.maxHeight = null;
      }
    }, 
  }
}
</script>
<style scoped>
ul {
  padding: 0;
  list-style: none;
}
.n2-nav{
  position: absolute;
  top: 48px;
  left: 0;
  background: #fff;
  z-index: 2;
  width: 100%;
}
.n3-itens-label{
  background: #e5e9f2;
  height: 48px;
  display: flex;
  align-items: center;
  width: 100%;
  padding: 0 16px 0 34px;
  position: relative;
  box-sizing: border-box;
}
.n3-itens-label::before{
  content: url(../../assets/img/arrow.svg);
  transform: rotate(90deg);
  position: absolute;
  left: 8px;
}
.iconOpenN3{
  position: absolute;
  right: 10px;
  transform: rotate(270deg);
}
.iconOpenN3.iconAtivo {
  transform: rotate(0deg);
}
.n3-listaSubSub {
  max-height: 0;
  overflow: hidden;
  transition: 0.4s;
}
.n3-itensSubSub:first-child{
  margin-top: 10px;
}
.n3-itensSubSub{
  padding: 10px;
}
.n3-itensSubSub:last-child{
  padding-bottom: 0;
}
.n3-itensSub{
  padding: 12px 16px;
  font-size: 14px;
  color: #787a7f;
}
.n3-listaSubSub.active3{
  transition: 0.5s;
}
@media(max-width: 1199px){
  .n3-itens-wrapper{
    height: 100vh;
  }
}
</style>
