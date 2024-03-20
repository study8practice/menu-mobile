<template>
  <header class="menu">
    <div class="btn-menu" @click="openMenu">
      <!-- Botão -->
      <div class="menu__btn">
        <span></span>
      </div>
    </div>

    <div class="lista-wrapper">
      <div class="topoMob">
        <button class="btn-close" @click="closeMenu">X</button>
      </div>

      <nav class="nav">
        <ul class="n1-lista">
          <li class="n1-itens" :class="{icon : n1.sub}" v-for="(n1, idN1) in n1menu" :key="idN1" @mouseenter="getIndice(idN1)">
            <span @click="screenWidthMob && n1.sub ? openN2(idN1) : false">{{n1.label}}</span>

            <div class="n2-wrapper">
              <ul class="n2-lista n2-listaSub n2-lista-1" v-if="n1.sub">
                <li v-show="n2NoChildren.length > 0">
                  <ul class=" n2NoChildren-lista n2-listaSub n2-lista-1" >
                    <li class="n2-itens" v-for="(n2No, i) in n2NoChildren" :key="i">
                      {{n2No.label}} 
                    </li>
                  </ul>
                </li>
                
                <li class="n2-itens" :class="{notChildren: n2.sub, empty: !n2.sub}" v-for="(n2, i) in n1.sub" :key="i" >
                  <template v-if="n2.sub">
                    {{n2.label}}
                  </template>
                  <div class="n3-wrapper" v-if="n2.sub">
                    <ul class="n3-lista n3-listaSub n3-lista-1" >
                      <li  class="n3-itens" v-for="(n3, i) in n2.sub" :n3="n3" :key="i" >
                        {{n3.label}}
                      </li>
                    </ul>
                  </div>
                </li>
              </ul>
            </div>
            <div  class="ativarN3" v-show="ativarN3 && this.indexMenu === idN1">
              <n2mob :n1Index="idN1" :ativo="ativarN3" @close-menu="closeMenu2"/>
            </div>
          </li>
        </ul>
      </nav>
    </div>
  </header>
</template>
<script>
import n2mob from './n2-itens-mob.vue';

export default {
  name: 'MainMenu',
  props: ['n1menu'],
  components: {
    n2mob
  },
  data() {
    return {
      n2NoChildren: [],
      ativarN3: false,
      indexMenu: 0,
      screenWidthDesktop: window.innerWidth > 1200,
      screenWidthMob: window.innerWidth < 1200
    }
  },
  methods: {
    getIndice(key){
      if(this.n1menu[key].sub){

        this.n2NoChildren = []

        this.n1menu[key].sub.forEach(m => {
          if(!m.sub){
            this.n2NoChildren.push(m)
          }
        });

      }
      
    },
    openMenu(){
      const nav = document.querySelector('.lista-wrapper');

      if(nav){
        nav.classList.toggle('ativo')
      }
    },
    closeMenu() {
      const nav = document.querySelector('.lista-wrapper');

      if(nav) {
        nav.classList.remove('ativo')
      }
    },
    openN2(id){
      this.ativarN3 = true;
      this.indexMenu = id;
    },
    closeMenu2(disable){
      this.ativarN3 = disable;
    }
  },
  mounted() {
    const n2 = document.querySelectorAll(".empty")
    const n2Array = Array.from(n2)
    n2Array.forEach(t => {
      t.parentElement.removeChild(t)
    })

    // https://devarthur.com/blog/como-remover-um-elemento-html-pelo-javascript
    // https://horadecodar.com.br/como-converter-uma-nodelist-em-array-com-javascript/

    const itens = document.querySelectorAll('.n3-itens');
    const itensArray = Array.from(itens)
    itensArray.forEach((item) => {

      if(item.innerText.toLowerCase() === "ver tudo"){
        item.classList.add('underlined')
      } 
    })
  }
}
</script>

<style scoped>
ul {
  list-style: none;
}
.menu{
  height: 75px;
  background: #fdd900;
  align-items: center;
}
@media(max-width: 1199px){
  
  .menu__btn {
    display: flex;
    align-items: center;
    position: relative;
    top: 20px;
    left: 20px;
  
    width: 26px;
    height: 26px;
  
    cursor: pointer;
    z-index: 1;
  }
  .menu__btn > span,
  .menu__btn > span::before,
  .menu__btn > span::after {
    display: block;
    position: absolute;
  
    width: 100%;
    height: 2px;
  
    background-color: #000;
  
    transition-duration: .25s;
  }
  .menu__btn > span::before {
    content: '';
    top: -8px;
  }
  .menu__btn > span::after {
    content: '';
    top: 8px;
  }
  .nav{
    background-color: #fff;
  }
  .lista-wrapper{
    position: absolute;
    top: 0;
    left: 0;
    z-index: 1;
    width: 100%;
  }
  .lista-wrapper{
    transition: .9s ease all;
		transform: translateX(-100%);
    width: 100%;
  }
  .lista-wrapper.ativo{
    transform: translateX(0%);
  }
  .n2-wrapper{
    display: none;
  }
  .topoMob{
    background-color: #fdd900;
    height: 48px;
    display: flex;
    justify-content: flex-end;
  }
  .btn-close{
    border: none;
    background: none;
    font-size: 25px;
    margin-right: 20px;
  }
  .n1-lista{
    padding: 16px 0;
  }
  .n1-itens{
    margin: 8px 16px 0;
    padding: 8px 0;
  }
  .n1-itens.icon::after{
    content: url(../../assets/img/arrow.svg);
    transform: rotate(270deg);
    position: absolute;
    right: 10px;
  }
}
@media(min-width: 1200px){
  .topoMob{
    display: none;
  }
  .btn-menu{
    display: none;
  }
  .lista-wrapper{
    max-width: 1200px;
    width: 100%;
    margin: 0 auto;
  }
  .n1-lista{
    display: flex;
    align-items: center;
    position: relative;
  }
  .n1-itens{
    cursor: pointer;
    height: 75px;
    width: 172px;
    display: flex;
    justify-content: center;
    align-items: center;
    color: #000;
  }
  .n1-itens:hover .n2-wrapper{
    display: block;
  }
  .n2-wrapper{
    display: none;
    background: #fff;

    max-width: 1200px;
    width: 100%;

    position: absolute;
    top: 75px;
    left: 0;
    height: 300px;
    overflow: auto;
  }
  .n2NoChildren-lista{
    max-width: 200px;
  }
  .n2-lista{
    display: grid;
    grid-template-columns: repeat(5, 200px);
    grid-gap: 36px;
    margin: 16px 24px;
  }
  .n2-itens{
    color: #000;
    font-size: 14px;
    font-weight: 700;
    margin-bottom: 8px;
  }
  .n3-wrapper{
    margin-top: 8px;
  }
  .n3-itens{
    font-weight: 400;
    margin-bottom: 8px;
  }
  .n3-itens.underlined{
    text-decoration: underline;
  }
}
</style>
