<template>
  <b-container class="border mt-2 rounded col-md-6 bgcolor">
    <b-form @submit.prevent="register" class="text-align mb-2">
      <b-form-group
        class="mt-2"
        id="input-group-1"
        label="أسم المستخدم:"
        label-for="input-1"
        description="يجب أن يتكون اسم المستخدم من 6-64 حرف, ارقام و @/./+/-/_ فقط."
      >
        <b-input-group>
          <b-input-group-prepend is-text>
            <b-icon icon="person-fill"></b-icon>
          </b-input-group-prepend>
          <b-form-input
            :class="[checkUsernameLength || alertStatus.name ? 'input' : '']"
            id="input-1"
            v-model="form.name"
            type="text"
            :title="title"
            :oninvalid="oninvalid"
            :oninput="oninput"
            required
            placeholder="ادخل الأسم"
          ></b-form-input>
        </b-input-group>
        <span v-if="checkUsernameLength || alertStatus.name">
          <b-alert show variant="danger" class="mt-1">
            <b-icon icon="exclamation-circle-fill" variant="danger"></b-icon>
            {{ alertMessage.name }}
          </b-alert></span
        >
      </b-form-group>

      <b-form-group
        id="input-group-2"
        label="البريد الالكتروني:"
        label-for="input-2"
      >
        <b-input-group>
          <b-input-group-prepend is-text>
            <b-icon icon="envelope"></b-icon>
          </b-input-group-prepend>
          <b-form-input
            :class="[
              alertStatus.emailFocus || alertStatus.email ? 'input' : ''
            ]"
            id="input-2"
            v-model="form.email"
            type="email"
            :title="title"
            @invalid="checkEmailValidity"
            @input="checkEmailValidity"
            required
            placeholder="ادخل البريد الالكتروني"
          ></b-form-input>
        </b-input-group>
        <span v-if="alertStatus.email">
          <b-alert show variant="danger" class="mt-1">
            <b-icon icon="exclamation-circle-fill" variant="danger"></b-icon>
            تم أخذ هذا البريد الالكتروني, ادخل احد اخر.
          </b-alert></span
        >
      </b-form-group>

      <b-form-group
        id="input-group-3"
        label="كلمة المرور:"
        label-for="input-3"
        description="يجب أن لا تقل عن 6 رموز!"
      >
        <b-input-group>
          <b-input-group-prepend is-text>
            <b-icon icon="lock-fill"></b-icon>
          </b-input-group-prepend>
          <b-form-input
            :class="[checkPasswordLength ? 'input' : '']"
            id="input-3"
            v-model="form.password"
            type="password"
            :title="title"
            :oninvalid="oninvalid"
            :oninput="oninput"
            required
            placeholder="ادخل كلمة المرور"
          ></b-form-input>
        </b-input-group>
        <span v-if="checkPasswordLength">
          <b-alert show variant="danger" class="mt-1">
            <b-icon icon="exclamation-circle-fill" variant="danger"></b-icon>
            يجب أن تتكون كلمة المرور من 6-100 رمز.
          </b-alert></span
        >
      </b-form-group>

      <b-form-group
        id="input-group-4"
        label="تأكيد كلمة المرور:"
        label-for="input-4"
      >
        <b-input-group>
          <b-input-group-prepend is-text>
            <b-iconstack>
              <b-icon stacked icon="lock-fill"></b-icon>
              <b-icon
                stacked
                icon="check"
                variant="white"
                shift-v="-4"
                scale="0.8"
              ></b-icon>
            </b-iconstack>
          </b-input-group-prepend>
          <b-form-input
            :class="[checkPasswordMatch ? 'input' : '']"
            id="input-4"
            v-model="form.password2"
            type="password"
            :title="title"
            :oninvalid="oninvalid"
            :oninput="oninput"
            required
            placeholder="ادخل كلمة المرور مرة اخرا للتأكيد"
          ></b-form-input>
        </b-input-group>
        <span v-if="checkPasswordMatch">
          <b-alert show variant="danger" class="mt-1">
            <b-icon icon="exclamation-circle-fill" variant="danger"></b-icon>
            يجب أن تكون كلمتا المرور متطابقة.
          </b-alert>
        </span>
      </b-form-group>

      <b-button type="submit" :disabled="checkSubmitBtn" variant="primary"
        >انشاء حساب</b-button
      >
    </b-form>
    <hr class="col-sm-4 col-6" />
    <div align="center" class="mb-2">
      هل لديك حساب مسبقاً؟
      <br />
      <b-link variant="primary" to="/login">تسجيل الدخول</b-link>
    </div>
  </b-container>
</template>

<script>
export default {
  data() {
    return {
      okRegister: false,
      title: ' يرجى ملىء ألحقل ',
      oninvalid: "setCustomValidity('يرجى ملىء ألحقل')",
      oninput: "setCustomValidity('')",
      submitBtn: false,
      form: {
        name: '',
        email: '',
        password: '',
        password2: ''
      },
      alertMessage: {
        name: 'يجب أن يتكون اسم المستخدم من 6-64 حرف, ارقام و @/./+/-/_ فقط.'
      },
      alertStatus: {
        name: false,
        email: false,
        emailFocus: false
      }
    }
  },
  computed: {
    checkUsernameLength: function() {
      if (this.form.name.length === 0) {
        return false
      } else if (this.form.name.length < 6 || this.form.name.length > 64) {
        return true
      } else return false
    },
    checkPasswordLength: function() {
      if (this.form.password.length === 0) {
        return false
      } else if (
        this.form.password.length < 6 ||
        this.form.password.length > 100
      ) {
        return true
      } else return false
    },
    checkPasswordMatch: function() {
      if (this.form.password2.length === 0) {
        return false
      } else if (this.form.password === this.form.password2) {
        return false
      } else if (this.form.password !== this.form.password2) {
        return true
      } else return false
    },
    checkSubmitBtn: function() {
      const mark =
        this.checkPasswordLength ||
        this.checkPasswordMatch ||
        this.checkUsernameLength
      return mark
    }
  },
  methods: {
    selectClass() {
      if (this.checkSubmitBtn) {
        return 'input'
      }
    },
    checkEmailValidity() {
      var email = document.getElementById('input-2')
      if (email.value === '') {
        email.setCustomValidity(' يرجى ملىء ألحقل ')
        this.alertStatus.emailFocus = true
      } else if (email.validity.typeMismatch) {
        this.alertStatus.emailFocus = true
        email.setCustomValidity(`يرجىء ادخال اميل صالح`)
      } else {
        this.alertStatus.emailFocus = false
        email.setCustomValidity('')
      }
    },
    register: async function() {
      try {
        const promise = await fetch(
          `${this.$store.state.tokenModel.url}register`,
          {
            method: 'POST',
            headers: {
              Accept: 'application/json',
              'Content-Type': 'application/json'
            },
            body: JSON.stringify({
              username: this.form.name,
              email: this.form.email,
              password: this.form.password,
              password2: this.form.password2
            })
          }
        )
        if (promise.ok) {
          this.$store.dispatch('tokenModel/getToken', {
            username: this.form.name,
            password: this.form.password
          })
          this.okRegister = true
        }
        const data = await promise.json()
        // console.log(Object.keys(data).length)
        // cos when they are both wrong and try to correct one they would still be both red if I do not do this.
        this.alertStatus.name = false
        this.alertStatus.email = false
        // to avoid showing the alerts when the registeration succeed, cos this part happens before the getToke func.
        if (!this.okRegister) {
          if (data.username) {
            this.alertMessage.name = 'تم أخذ هذا الأسم,  ادخل اسم اخر.'
            this.alertStatus.name = true
          }
          if (data.email) {
            this.alertStatus.email = true
          }
        }
      } catch (err) {
        console.log(err)
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.text-align {
  text-align: right;
}
.bgcolor {
  background-color: lightblue;
}
.input:focus {
  outline: none !important;
  border-color: rgb(219, 24, 24);
  box-shadow: 0 0 5px red;
}
</style>
