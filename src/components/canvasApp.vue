<template>
  <div class="canvas">
    <header>
        <img src="../assets/Logo.svg" alt="За стеклом">
        <button @click='SendToServer' type="btn" class='btn' name="button">Формулировка идеи </button>
    </header>
    <main>
      <input type="text" v-model="name" class='title'  name="" value="" placeholder="Введите название вашей идеи">
      <div class="Product">
        <p>ВАШ ПРОДУКТ</p>
        <div class="Products">
          <div class='top'>

            <div class="left">
              <!-- {{data.Products[0].BenefitsOfTheProduct}} -->
              <div v-for='elem in data.Products[0].BenefitsOfTheProduct' class="block">
                <p>{{elem.name}}</p>
                <p>{{elem.Description.length > 0 ? '...' : '' }}</p>
              </div>
              <div @click='add("Products[0].BenefitsOfTheProduct")' class="add">+</div>
            </div>

            <div class="right">
              <div v-for='elem in data.Products[0].differences' class="block">
                <p>{{elem.name}}</p>
                <p>{{elem.Description.length > 0 ? '...' : '' }}</p>

              </div>
              <div @click='add("Products[0].differences")' class="add">+</div>
            </div>
        </div>
        <div class='notop'>
          <div class="center">
            <div v-for='elem in data.Products[0].HowTheProductWillSolveTheProblem' class="blockC">
              <p>{{elem.name}}</p>
              <p>{{elem.Description.length > 0 ? '...' : '' }}</p>

            </div>
            <div @click='add("Products[0].HowTheProductWillSolveTheProblem")' class="addC">+</div>
          </div>
        </div>

        </div>
      </div>
      <div class="Client">
        <p>ВАШ КЛИЕНТ</p>
        <div class="Clients">
          <div class='top'>

            <div class="left">
              <div v-for='elem in data.Clients[0].clientDescription' class="block">
                <p>{{elem.name}}</p>
                <p>{{elem.Description.length > 0 ? '...' : '' }}</p>

              </div>
              <div @click='add("Clients[0].clientDescription")' class="add">+</div>
            </div>

            <div class="right">
              <div v-for='elem in data.Clients[0].problemsFromTheClient' class="block">
                <p>{{elem.name}}</p>
                <p>{{elem.Description.length > 0 ? '...' : '' }}</p>

              </div>
              <div @click='add("Clients[0].problemsFromTheClient")' class="add">+</div>
            </div>
          </div>
          <div class='notop'>
            <div class="left">
              <div v-for='elem in data.Clients[0].whatProblemIsBeingSolvedNow' class="block">
                <p>{{elem.name}}</p>
                <p>{{elem.Description.length > 0 ? '...' : '' }}</p>

              </div>
              <div @click='add("Clients[0].whatProblemIsBeingSolvedNow")' class="add">+</div>
            </div>

            <div class="right">
              <div v-for='elem in data.Clients[0].whatTasksTheClientSolves' class="block">
                <p>{{elem.name}}</p>
                <p>{{elem.Description.length > 0 ? '...' : '' }}</p>

              </div>
              <div @click='add("Clients[0].whatTasksTheClientSolves")' class="add">+</div>
            </div>
          </div>
        </div>
      </div>
    </main>
    <div @click='VisibleFalse' v-if='isVisible' class="modal">

    </div>
    <div  v-if='isVisible' class="InputsModal">
      <p class='title'>{{title}}</p>
      <div class="template">
      <p>Тема стикера</p>
      <input class='input1' type="text" name="ThemeSticker" v-model='ThemeSticker' value="">
      </div>
      <div class="template">
      <p>Описание</p>
      <input class='input2'type="text" name="DescriptionSticker" v-model='DescriptionSticker' value="">
      </div>
      <button @click='SaveSticer' class="btn" name="button">Сохранить</button>
    </div>
  </div>

</template>

<script>
import $ from "jquery"

export default {
  name: 'canvas',
  props: {
    // msg: String
  },
  data(){
    return{
      data: {
        "Products":
           [{
              "BenefitsOfTheProduct": [],
              "differences": [],
              "HowTheProductWillSolveTheProblem": []
            }],
        "Clients":
            [{
                "clientDescription":[],
                "problemsFromTheClient":[],
                "whatProblemIsBeingSolvedNow":[],
                "whatTasksTheClientSolves":[]
            }]
        },

        isVisible: false,
        title: '',
        name: '',
        ThemeSticker: '',
        DescriptionSticker: '',
        RoutInS: ''
       }
   },

  methods: {
    SendToServer(){
      let vm = this;

        $.ajax({
          type: "POST",
          url: "http://91.201.54.66:3000/save",
          acync: false,
          crossDomain: true,
          data: {
            data: JSON.stringify(vm.data),
            title: JSON.stringify(vm.name)
          },
          success: function(data) {
            console.log(data);
          },
          error: function(error) {}
      });
    },
    SaveSticer(){
      let param = this.data[this.RoutInS.split('[')[0]][0][this.RoutInS.split('.')[1]]
      param.push({'Description': this.DescriptionSticker, 'name': this.ThemeSticker})
      this.isVisible = false;
      this.DescriptionSticker = '';
      this.ThemeSticker = '';
      // param.push("Description": this.DescriptionSticker, "name": this.ThemeSticker);
    },
    VisibleFalse(){
      this.isVisible = false;
    },
    add(data){
      this.RoutInS = data;
      if (data.split('.')[1] == 'BenefitsOfTheProduct') {
        this.title = 'В чем выгоды вашего продукта'
      }
      else if (data.split('.')[1] == 'differences') {
        this.title = 'Отличия от конкурентов'
      }
      else if (data.split('.')[1] == 'HowTheProductWillSolveTheProblem') {
        this.title = 'Как продукт решает проблему'
      }
      else if (data.split('.')[1] == 'clientDescription') {
        this.title = 'Описание клиента'
      }
      else if (data.split('.')[1] == 'problemsFromTheClient') {
        this.title = 'Какие проблемы возникают у клиента'
      }
      else if (data.split('.')[1] == 'whatProblemIsBeingSolvedNow') {
        this.title = 'Как проблема решается сейчас'
      }
      else if (data.split('.')[1] == 'whatTasksTheClientSolves') {
        this.title = 'Какие задачи решает клиент'
      }
      this.isVisible = !this.isVisible
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
header{
  width: 100%;
  height: 54px;
  display: flex;
  justify-content: space-between;
  padding-left: 30px;
  padding-right: 30px;
  align-items: center;
  box-shadow: 0 -15px 10px 10px #000;
  background: #fff;

}
main{
  width: 100%;
  display: flex;
  justify-content: space-around;
  align-items: baseline;
}
.btn{
  color: #fff;
  background: linear-gradient(220.46deg,#6f27e2 -2.59%,#8430cf 65.86%);
  cursor: pointer;
  text-align: center;
  font: inherit;
  padding: 5px 10px;
  border-radius: 3px;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  width: 165px;
  height: 34px;
  border: 0;
  font-size: 13px;
}
.Products{
  width: calc(50vw - 100px);
  height: calc(100vh - 54px - 68px - 20px);
  background: #fff;
}
.Clients{
  width: calc(50vw - 100px);
  height: calc(100vh - 54px - 68px - 20px);
  background: #fff;
}
.Product p, .Client p {
  color: rgba(61,72,83,.6);
  font-size: 1.2rem;
  font-weight: normal;
}
.top{
  width: 100%;
  height: 50%;
  border-bottom: 2px solid #e4e4e4;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: nowrap;
}
.notop{
  height: 50%;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: nowrap;

}
.left{
  border-right: 2px solid #e4e4e4;

}
.right{

}
.left:hover .add{
  opacity: 1;
  visibility: visible;
}
.right:hover .add{
  opacity: 1;
  visibility: visible;
}
.center:hover .addC{
  opacity: 1;
  visibility: visible;
}
.right, .left {
  height: 100%;
  width: 50%;
  padding: 10px;
  overflow: auto;
  display: flex;
  justify-content: flex-start;
  align-items: flex-start;
  flex-wrap: wrap;
}
.center{
  height: 100%;
  width: 100%;

  display: flex;
  justify-content: flex-start;
  align-items: flex-start;
  flex-wrap: wrap;

  padding: 10px;

  overflow: auto;
}
.add{
  width: 48%;
  height: 48%;
  margin: 1%;
  border: 2px dashed #bae2e2;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #bae2e2;
  font-size: 40px;
  cursor: pointer;
  opacity: 0;
  visibility: hidden;
  transition:  0.3s opacity;
}
.addC{
  width: 23.5%;
  height: 48%;
  margin: 0.7%;
  border: 2px dashed #bae2e2;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #bae2e2;
  font-size: 40px;
  cursor: pointer;
  opacity: 0;
  visibility: hidden;
  transition:  0.3s opacity;
}
.block{
  width: 48%;
  height: 48%;
  margin: 1%;
  background: #bae2e2;
  font-size: 2vh!important;

}
.blockC{
  width: 23.5%;
  height: 48%;
  margin: 0.7%;
  background: #bae2e2;
  font-size: 2vh!important;

}
.modal{
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  position: absolute;
  background: #0004;
  /* visibility: hidden; */
  /* opacity: 0; */
}
.InputsModal{
  width: 700px;
  height: 400px;
  background: #fff;
  position: absolute;
  top: calc(50vh  - 200px);
  left: calc(50vw - 350px);
  border-radius: 10px;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
.template{
  width: 80%;
}
.template p {
  text-align: left;
  margin-left: 20%;
}
.input1, .input2{
  width: 340px;
  height: 30px;
  border-radius: 2px;
  border: solid 1px #80868d;
}
.input2{
  height: 80px;
}
.title{
  position: absolute;
  width: 300px;
  height: 40px;
  border-radius: 100px;
  border: 0;
  background: #fff;
  margin-top: 14px;
  text-align: center;
  font-size: 15px;
}
</style>
