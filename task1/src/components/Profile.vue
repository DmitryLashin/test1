<template>
    <h1> Профиль пользователя </h1>
    <input v-model="name" required="true" placeholder="Имя" class="inputElementStyle">
    <input v-model="secondName" placeholder="Фамилия" class="inputElementStyle">
    <input v-model="email" placeholder="e-mail" class="inputElementStyle" v-on:change="emailCheck">
    <input v-phone v-model="phoneNumber" placeholder="Телефон" v-on:keypress="phoneKeyFilter" class="inputElementStyle">
    <input v-model="birthDate" maxlength="10" placeholder="Дата рождения" v-on:keydown="birthDateInput" class="inputElementStyle">
    <br>
    <button v-on:click="checkInputData" class="button">Сохранить</button>
    <br>
    <br>
    <input type="file" accept="image/jpeg" v-on:change="previewFile" class="inputFileStyle"><br />
    <div v-if="showProfilePicture">
      <img src="" height="200">
    </div>
    <div v-if="showOkMessage">
        Данные успешно сохранены
    </div>
</template>

<script lang="ts">
import { ButtonHTMLAttributes, Directive } from 'vue'
import { Options, Vue } from 'vue-class-component'

@Options({ props: {} })

export default class Profile extends Vue {
    name!: string
    secondName!: string
    email!: string
    phoneNumber!: string
    birthDate = ''
    showOkMessage = false
    showProfilePicture = false
    profileImage = ''

    public checkInputData (): void {
      this.showOkMessage = true
    }

    public phoneKeyFilter (e: KeyboardEvent): void {
      if (!/[0-9+ -]/.test(e.key) && e.key !== 'Backspace') {
        e.preventDefault()
      }
    }

    public birthDateInput (e: KeyboardEvent): void {
      if (!/[0-9]/.test(e.key) && e.key !== 'Backspace') {
        e.preventDefault()
      }

      if ((this.birthDate.length === 2 || this.birthDate.length === 5) && e.key !== 'Backspace') {
        e.preventDefault()
        this.birthDate += '.' + e.key
      }
    }

    public emailCheck (e: Event): void {
      if (this.email && !/^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/.test(this.email)) {
        alert('Укажите корректный адрес электронной почты')
      }
    }

    public previewFile (e: Event): void {
      const eventTarget = e.target as HTMLInputElement
      const preview = document.querySelector('img')

      if (eventTarget && eventTarget.files) {
        const file = eventTarget.files[0]
        const reader = new FileReader()

        reader.addEventListener(
          'load',
          () => {
            const base64result = reader.result
            if (base64result) {
              this.profileImage = base64result.toString()
              if (preview) {
                preview.src = base64result.toString()
              }
            }
          },
          false
        )

        if (file) {
          reader.readAsDataURL(file)
          this.showProfilePicture = true
        }
      }
    }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.inputElementStyle {
  width: 15%;
  height: calc(2.25rem+2px);
  padding: 0.375rem 0.75rem;
  font-size: 1rem;
  font-weight: 400;
  line-height: 1.5;
  display: block;
  border: 1px solid #bdbdbd;
  border-radius: 0.25rem;
  margin-left: auto;
  margin-right: auto;
  margin-top: 5px;
}

.button {
    background-color: #e1e6e1;
    border: none;
    color: rgb(8, 8, 7);
    padding: 15px 32px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
    border-radius: 0.25rem;
    margin-top: 5px;
}
.inputFileStyle {
  background-color: #e1e6e1;
    border: none;
    color: rgb(8, 8, 7);
    padding: 15px 32px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
    border-radius: 0.25rem;
    margin-top: 5px;
}
</style>
