<template>
  <div :class="{hidden: modal}">
    <form class="theForm" id="theForm" @submit.prevent="onSubmit()">
      <label class="theForm__section theForm__section_type_required">Ваш филиал</label>
      <select v-model="selectCity" aria-placeholder="Выберите город" :disabled= "selectCity === 'Онлайн'" @:change="formValidate()">
        <option disabled value="">Выберите город</option>
        <option v-for="city of cities" :key="city.id" :value="city.id" >{{city.title}}</option>
      </select>
      <div class="section__wrap">
        <input type="checkbox" v-model="selectCity" true-value="Онлайн" false-value="" @:change="formValidate()">
        <label>Онлайн</label>
      </div>
      <label class="theForm__section theForm__section_type_required">Тема обращения</label>
        <div class="section__wrap">
          <input type="radio" v-model="chosenReason" value="Недоволен качеством услуг" @:change="formValidate()">
          <label>Недоволен качеством услуг</label>
        </div>
        <div class="section__wrap">
          <input type="radio" v-model="chosenReason" value="Расторжение договора" @:change="formValidate()">
          <label>Расторжение договора</label>
        </div>
        <div class="section__wrap">
          <input type="radio" v-model="chosenReason" value="Не приходит письмо активации на почту" @:change="formValidate()"> 
          <label>Не приходит письмо активации на почту</label>
        </div>
        <div class="section__wrap">
          <input type="radio" v-model="chosenReason" value="Не работает личный кабинет" @:change="formValidate()">
          <label>Не работает личный кабинет</label>
        </div>
        <input type="text" placeholder="Другое" v-model="chosenReason" @:change="formValidate()">
      
      <label class="theForm__section theForm__section_type_required">Описание проблемы</label> 
      <textarea v-model="description" placeholder="Введите текст" @:keyup="formValidate()"></textarea>
      
      <label class="theForm__section">Загрузка документов</label>
        <p class="section__descr">Приложите, пожалуйста, полноэкранный скриншот. Это поможет быстрее решить проблему.</p>
        <input type="file" accept="image/jpeg, image/gif, image/png">
      
      <button type="submit" class="form__button form__button_type_disabled" id="button-sabmit" :disabled="buttonDisabled">Отправить</button>
    </form>
    <div class="modal" :class="{modal_isActive: modal}">
      <div class="modal__wrap">
        <div class="modal__content">
          <h2>Заявка отправлена.</h2>
          <img src="../assets/success_img.png">
          <img src="../assets/exit-button.png" class="modal__button" @:click="closeModal()">
        </div> 
      </div> 
    </div>
  </div>
</template>

<script>
const axios = require('axios').default;

export default {
  name: 'TheForm',
  data() {
    return {
      selectCity: '',
      cities: [],
      chosenReason: '',
      description: '',
      errors: [],
      buttonDisabled: true,
      modal: false
    }
  },
  created() {
    axios.get('https://60254fac36244d001797bfe8.mockapi.io/api/v1/city')
    .then(res => this.cities = res.data)
    .catch(err => console.log(err))
  },
  
  methods: {
    formValidate() {
      if (this.selectCity && this.chosenReason && this.description) {
        this.buttonDisabled = false
      } else {
        this.buttonDisabled = true
      }
    },

    onSubmit() {
      axios.post('https://60254fac36244d001797bfe8.mockapi.io/api/v1/send-form')
      .then(res => { 
        if (res.data.success){
          this.modal = true
          } else {
            alert('Ошибка отправки заявки')
          }
      })
      .catch(err => console.log(err))
    
    },

    closeModal() {
      this.modal = false
    }
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.theForm {
  display: flex;
  flex-direction: column;
  max-width: 1300px;
  justify-content: center;
  border: solid 1px grey;
  align-items: flex-start;
  padding: 30px;
  gap: 10px
}

.theForm__section_type_required::after {
  content: ' *';
  color: red;
}

.section__wrap {
  display: flex;
}

.modal {
  position: fixed;
  z-index: 2;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  transition: opacity 0.3s ease;
  display: none;
}

.modal_isActive {
  display: block;
}

.modal__wrap {
  position: relative;
  height: inherit;
}

.modal__content {
  width: 300px;
  margin: 0px auto;
  padding: 20px 30px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.modal__button {
  position: absolute;
  max-width: 50px;
  top: -50px;
  right: -50px;
  cursor: pointer;
}

.hidden {
  overflow: hidden;
}

</style>
