<template>
  <div :class="{hidden: modal}">
    <form class="form" id="form" @submit.prevent="onSubmit()">
      <div class="form__section form__section_type_list">
        <label class="form__title form__title_type_required" for="selectCity-list">Ваш филиал</label>
        <select
        class="form__input form__input_type_list list"
        id="selectCity-list"
        v-model="selectCity"
        aria-placeholder="Выберите город" :disabled= "selectCity === 'Онлайн'" 
        v-on:change="formValidate()"
        >
          <option class="list__item" disabled value="">Выберите город</option>
          <option class="list__item" v-for="city of cities" :key="city.id" :value="city.id" >{{city.title}}</option>
        </select>
      </div>
      <div class="form__section form__section_type_check">
        <input
        class="form__input form__input_type_check"
        type="checkbox" 
        v-model="selectCity" 
        true-value="Онлайн" 
        false-value="" 
        v-on:change="formValidate()" 
        id="selectCity-check"
        >
        <label class="form__label" for="selectCity-check">Онлайн</label>
      </div>
      <div class="form__sections form__sections_type_radio">
              <label class="form__title form__title_type_required">Тема обращения</label>
        <div class="form__section form__section_type_radio">
          <input 
          class="form__input form__input_type_radio"
          type="radio" 
          v-model="chosenReason" 
          value="Недоволен качеством услуг" 
          v-on:change="formValidate()"
          id="radio-1"
          >
          <label class="form__label" for="radio-1" >Недоволен качеством услуг</label>
        </div>
        <div class="form__section form__section_type_radio">
          <input
          class="form__input form__input_type_radio"
          type="radio"
          v-model="chosenReason"
          value="Расторжение договора"
          v-on:change="formValidate()"
          id="radio-2"
          >
          <label class="form__label" for="radio-2">Расторжение договора</label>
        </div>
        <div class="form__section form__section_type_radio">
          <input
          class="form__input form__input_type_radio"
          type="radio"
          v-model="chosenReason"
          value="Не приходит письмо активации на почту"
          v-on:change="formValidate()"
          id="radio-3"
          > 
          <label class="form__label" for="radio-3" >Не приходит письмо активации на почту</label>
        </div>
        <div class="form__section form__section_type_radio">
          <input 
          class="form__input form__input_type_radio"
          type="radio" 
          v-model="chosenReason" 
          value="Не работает личный кабинет" 
          v-on:change="formValidate()"
          id="radio-4"
          >
          <label class="form__label" for="radio-4" >Не работает личный кабинет</label>
        </div>
      </div>
        <input
        class="form__input form__input_type_text"
        type="text"
        placeholder="Другое" 
        v-model="chosenReason"
        v-on:change="formValidate()"
        >

      <div class="form__section form__section_type_textarea">
        <label class="form__title form__title_type_required" for="problemDescription">Описание проблемы</label> 
        <textarea
          class="form__input form__input_type_textarea"
          v-model="description"
          placeholder="Введите текст" 
          v-on:keyup="formValidate()"
          id="problemDescription"
        ></textarea>
      </div>
      
      <div class="form__section form__section_type_file">
        <label class="form__title" for="downloadFile">Загрузка документов</label>
        <p class="form__subtitle">Приложите, пожалуйста, полноэкранный скриншот. Это поможет быстрее решить проблему.</p>
        <input
          class="form__input form__input_type_file"
          type="file"
          accept="image/jpeg, image/gif, image/png"
          id="downloadFile"
        >
      </div>
      <button 
        class="form__button"
        type="submit"  
        id="button-submit" 
        :disabled="buttonDisabled"
      >
        Отправить
      </button>

    </form>
    <div class="modal" :class="{modal_isActive: modal}">
      <div class="modal__wrap">
        <div class="modal__content">
          <h2 class="modal__title">Заявка отправлена.</h2>
          <img class="modal__success-img" src="../assets/success_img.png">
          <img 
          class="modal__button modal__button_type_close" 
          src="../assets/exit-button.png"  
          v-on:click="closeModal()"
          >
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
.form {
  display: flex;
  flex-direction: column;
  max-width: 1300px;
  justify-content: center;
  border: solid 1px #e1e1e1;
  align-items: flex-start;
  padding: 35px;
  gap: 9px;
  letter-spacing: .4px;
}

.form__title {
  font-size: 15px;
}

.form__subtitle {
  font-size: 13px;
  margin-top: 11px;
  max-width: 350px;
}

.form__input {
  letter-spacing: .4px;
}

.form__input_type_list {
  background-color: white;
  border: 1px solid #e1e1e1;
  padding: 7px;
  cursor: pointer;
  border-radius: 2px;
}

.form__input_type_check,
.form__input_type_radio {
  position: absolute;
  z-index: -1;
  opacity: 0;
}

.form__input_type_check+label,
.form__input_type_radio+label {
  display: inline-flex;
  align-items: center;
  user-select: none;
}

.form__input_type_check+label::before {
  content: '';
  display: inline-block;
  color: transparent;
  width: 22px;
  height: 22px;
  border: 1px solid black;
  flex-shrink: 0;
  flex-grow: 0;
  border-radius: 1px;
  margin-right: 12px;
  background-repeat: no-repeat;
  background-position: center center;
  background-size: 50% 50%;
}

.form__input_type_check:checked+label::before {
  background-image: url("../assets/checkbox.png");
}

.form__input_type_radio+label::before {
  content: '';
  display: inline-block;
  width: 20px;
  height: 20px;
  flex-shrink: 0;
  flex-grow: 0;
  border: 1px solid black;
  border-radius: 50%;
  margin-right: 10px;
  background-repeat: no-repeat;
  background-position: center center;
  background-size: 50% 50%;
}

.form__input_type_text {
  margin-top: 6px;
  min-width: 26%;
  padding: 6px; 
}

.form__input_type_radio:checked+label::before {
  background-image: url("../assets/dot.png");
}

.form__input_type_textarea {
  resize: none;
  outline: none;
  min-height: 115px;
}

.form__title_type_required::after {
  content: ' *';
  color: red;
}

.form__section {
  display: flex;
}

.form__section_type_check {
  margin-top: 9px;
}

.form__sections_type_radio {
  margin-top: 28px;
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.form__section_type_list {
  flex-direction: column;
  min-width: 28%;
  gap: 9px;
}

.form__section_type_textarea {
  display: flex;
  flex-direction: column;
  margin-top: 30px;
  width: 100%;
  gap: 10px;
}

.form__section_type_file {
  display: flex;
  flex-direction: column;
  margin-top: 25px;
}

.form__label {
  cursor: pointer;
  font-size: 13px;
}

.form__button {
  margin-top: 21px;
  text-transform: uppercase;
  background-color: #ff9767;
  color: white;
  padding: 8px;
  outline: none;
  border: none;
  min-width: 110px;
}

.form__button:disabled {
  background-color: #e2e2e2;
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
