<template>
  <div class="canvas">
    <header>
        <img src="../assets/Logo.svg" alt="logo">
        <button @click='SendDataToTheServer' type="btn" class='btn' name="button">Отправить идею</button>
    </header>
    <main>
      <input type="text" v-model="name" class='NameOfYourIdea'  name="" value="" placeholder="Введите название вашей идеи">
      <div class="Product">
        <p>ВАШ ПРОДУКТ</p>
        <div class="Products">
          <div class='top'>

            <div class="left">
              <p class="Description">В чем выгоды вашего продукта</p>
              <div v-for='elem in data[0].BenefitsOfTheProduct' class="block BenefitsOfTheProduct">
                <img @click='del({"elem":elem,"arr":"BenefitsOfTheProduct"})' class='del' src="../assets/delete.png" alt="">

                <p>{{elem.Description.length > 0 ? '...' : '' }}</p>
              </div>
              <div @click='AddSticker("BenefitsOfTheProduct")' class="add BenefitsOfTheProductAdd">+</div>
            </div>

            <div class="right">
              <p class="Description">Отличия от конкурентов</p>
              <div v-for='elem in data[0].differences' class="block differences">
                <img @click='del({"elem":elem,"arr":"differences"})' class='del' src="../assets/delete.png" alt="">

                <p>{{elem.name}}</p>
                <p>{{elem.Description.length > 0 ? '...' : '' }}</p>

              </div>
              <div @click='AddSticker("differences")' class="add differencesAdd">+</div>
            </div>
        </div>

        <div class='notop'>
          <div class="center">
            <p class="Description">Как продукт решает проблему</p>

            <div v-for='elem in data[0].HowTheProductWillSolveTheProblem' class="blockC HowTheProductWillSolveTheProblem">
              <img @click='del({"elem":elem,"arr":"HowTheProductWillSolveTheProblem"})' class='del' src="../assets/delete.png" alt="">

              <p>{{elem.name}}</p>
              <p>{{elem.Description.length > 0 ? '...' : '' }}</p>

            </div>
            <div @click='AddSticker("HowTheProductWillSolveTheProblem")' class="addC HowTheProductWillSolveTheProblemAdd">+</div>
          </div>
        </div>

        </div>
      </div>

      <div class="Client">
        <p>ВАШ КЛИЕНТ</p>
        <div class="Clients">
          <div class='top'>

            <div class="left">
              <p class="Description">Описание клиента</p>

              <div v-for='elem in data[0].clientDescription' class="block clientDescription">
                <img @click='del({"elem":elem,"arr":"clientDescription"})' class='del' src="../assets/delete.png" alt="">

                <p>{{elem.name}}</p>
                <p>{{elem.Description.length > 0 ? '...' : '' }}</p>

              </div>
              <div @click='AddSticker("clientDescription")' class="add clientDescriptionAdd">+</div>
            </div>

            <div class="right">
              <p class="Description">Какие проблемы возникают у клиента</p>
              <div v-for='elem in data[0].problemsFromTheClient' class="block problemsFromTheClient">
                <img @click='del({"elem":elem,"arr":"problemsFromTheClient"})' class='del' src="../assets/delete.png" alt="">

                <p>{{elem.name}}</p>
                <p>{{elem.Description.length > 0 ? '...' : '' }}</p>

              </div>
              <div @click='AddSticker("problemsFromTheClient")' class="add problemsFromTheClientAdd">+</div>
            </div>
          </div>
          <div class='notop'>
            <div class="left">
              <p class="Description">Как проблема решается сейчас</p>
              <div v-for='elem in data[0].whatProblemIsBeingSolvedNow' class="block whatProblemIsBeingSolvedNow">
                <img @click='del({"elem":elem,"arr":"whatProblemIsBeingSolvedNow"})' class='del' src="../assets/delete.png" alt="">

                <p>{{elem.name}}</p>
                <p>{{elem.Description.length > 0 ? '...' : '' }}</p>

              </div>
              <div @click='AddSticker("whatProblemIsBeingSolvedNow")' class="add whatProblemIsBeingSolvedNowAdd">+</div>
            </div>

            <div class="right">
              <p class="Description">Какие задачи решает клиент</p>
              <div v-for='elem in data[0].whatTasksTheClientSolves' class="block whatTasksTheClientSolves">
                <img @click='del({"elem":elem,"arr":"whatTasksTheClientSolves"})' class='del' src="../assets/delete.png" alt="">

                <p>{{elem.name}}</p>
                <p>{{elem.Description.length > 0 ? '...' : '' }}</p>

              </div>
              <div @click='AddSticker("whatTasksTheClientSolves")' class="add whatTasksTheClientSolvesAdd">+</div>
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
      <input class='input1' type="text" name="ThemeSticker" v-model='ThemeSticker' value="" maxlength="25">
      </div>
      <div class="template">
      <p>Описание</p>
      <!-- <input class='input2'type="text" name="DescriptionSticker" v-model='DescriptionSticker' value=""> -->
      <textarea  class='input2'name="name" rows="8" cols="80" v-model='DescriptionSticker' ></textarea>
      </div>
      <button @click='SaveSticer' class="btn" name="button">Сохранить</button>
    </div>
  </div>

</template>

<script>
import $ from "jquery";
import swal from 'sweetalert';



export default {
  name: 'canvas',
  props: {
    // msg: String
  },
  data(){
    return{
      data: [{
              "BenefitsOfTheProduct": [],
              "differences": [],
              "HowTheProductWillSolveTheProblem": [],
              "clientDescription":[],
              "problemsFromTheClient":[],
              "whatProblemIsBeingSolvedNow":[],
              "whatTasksTheClientSolves":[]
            }],

        isVisible: false,
        title: '',
        name: '',
        ThemeSticker: '',
        DescriptionSticker: '',
        RoutInS: ''
       }
   },

  methods: {
    del(elem){
      // console.log(elem);

      // console.log(this.data[0][elem.arr]);
      // console.log(elem.elem);

      // console.log(this.data[0][elem.arr].includes(elem.elem))

      for (var i = 0; i < this.data[0][elem.arr].length; i++) {
        console.log(JSON.stringify(this.data[0][elem.arr][i].Description));
        console.log(JSON.stringify(this.data[0][elem.arr][i].Name));
        console.log(JSON.stringify(elem.elem['Name']));
        if (JSON.stringify(this.data[0][elem.arr][i].Name) == elem.elem.Name && JSON.stringify(this.data[0][elem.arr][i].Description) == elem.elem.Description) {
          console.log('eeee');
        }
        // console.log(this.data[0][elem.arr][i].Description);
      }

    },
    SendDataToTheServer(){
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
            if (data == 'OK') {
              swal("Отправленно", "Теперь мы можем увидеть вашу идею", "success");
            } else {
              swal("Ошибка сервека", "Пинайте vk.com/nik19ta ", "error");

            }
          },
          error: function(error) {
            swal("Ошибка сервека", "Пинайте vk.com/nik19ta ", "error");

          }
      });
    },
    SaveSticer(){


      let param = this.data[0][this.RoutInS]

      console.log(param);

      param.push({'Description': this.DescriptionSticker, 'name': this.ThemeSticker})
      this.isVisible = false;
      this.DescriptionSticker = '';
      this.ThemeSticker = '';
    },
    VisibleFalse(){
      this.isVisible = false;
    },
    AddSticker(data){
      this.RoutInS = data;
      console.log(data);
      if (data == 'BenefitsOfTheProduct') {
        this.title = 'В чем выгоды вашего продукта'
      }
      else if (data == 'differences') {
        this.title = 'Отличия от конкурентов'
      }
      else if (data == 'HowTheProductWillSolveTheProblem') {
        this.title = 'Как продукт решает проблему'
      }
      else if (data == 'clientDescription') {
        this.title = 'Описание клиента'
      }
      else if (data == 'problemsFromTheClient') {
        this.title = 'Какие проблемы возникают у клиента'
      }
      else if (data == 'whatProblemIsBeingSolvedNow') {
        this.title = 'Как проблема решается сейчас'
      }
      else if (data == 'whatTasksTheClientSolves') {
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

.blockC:hover .del {
  opacity: 1;
  visibility: visible;
}
.block:hover .del {
  opacity: 1;
  visibility: visible;
}

.right, .left {
  height: 100%;
  position: relative;
  padding-top: 30px!important;
  overflow-x: hidden;
  overflow-y: auto;
  width: 50%;
  padding: 10px;
  display: flex;
  justify-content: flex-start;
  align-items: flex-start;
  flex-wrap: wrap;
}
.center{
  position: relative;
  padding-top: 30px!important;
  overflow-x: hidden;
  overflow-y: auto;
  height: 100%;
  width: 100%;

  display: flex;
  justify-content: flex-start;
  align-items: flex-start;
  flex-wrap: wrap;

  padding: 10px;
}
.add{
  width: 48%;
  height: 48%;
  margin: 1%;
  /* border: 2px dashed #bae2e2; */
  display: flex;
  justify-content: center;
  align-items: center;
  /* color: #bae2e2; */
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
  /* border: 2px dashed #bae2e2; */
  display: flex;
  justify-content: center;
  align-items: center;
  /* color: #bae2e2; */
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
  position: relative;
  cursor: pointer;
}
.block p, .blockC p {
  font-size: 14px;
  color: #444444;
}

.blockC{
  width: 23.5%;
  height: 48%;
  margin: 0.7%;
  position: relative;
  cursor: pointer;

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
  font-family: sans-serif;
  color: #2c3e50;
  font-size: 13px;
  padding-left: 10px;
  padding-right: 10px;

}
.input2{
  height: 80px;
  vertical-align: top;
  resize: none;

}
.NameOfYourIdea{
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
.Description{
  font-size: 13px!important;
  text-align: center;
  width: 100%;
  position: absolute;
  top: -5px;
  /* height: 5px; */
}

.BenefitsOfTheProduct{
  background: #bae2e2;
}
.differences{
  background: #d5e9fa;
}
.HowTheProductWillSolveTheProblem{
  background: #ffaa9f;
}
.clientDescription{
  background: #ffe8da;
}
.problemsFromTheClient{
  background: #a5affb;
}
.whatProblemIsBeingSolvedNow{
  background: #e5e7fa;
}
.whatTasksTheClientSolves{
  background: #ffc7a6;
}

/*  */
/*  */
/*  */
/*  */

.BenefitsOfTheProductAdd{
  border: 2px dashed #bae2e2;
  color: #bae2e2;
}
.differencesAdd{
  border: 2px dashed #d5e9fa;
  color: #d5e9fa;

}
.HowTheProductWillSolveTheProblemAdd{
  border: 2px dashed #ffaa9f;
  color: #ffaa9f;

}
.clientDescriptionAdd{
  border: 2px dashed #ffe8da;
  color: #ffe8da;

}
.problemsFromTheClientAdd{
  border: 2px dashed #a5affb;
  color: #a5affb;

}
.whatProblemIsBeingSolvedNowAdd{
  border: 2px dashed #e5e7fa;
  color: #e5e7fa;

}
.whatTasksTheClientSolvesAdd{
  border: 2px dashed #ffc7a6;
  color: #ffc7a6;

}

.del{
  position: absolute;
  width: 15px;
  top: 5px;
  right: 5px;
  visibility: hidden;
  opacity: 0;
}
</style>
