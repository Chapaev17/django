<template>
  <div class="registration">
    <div class="modal-reg">
      <b-modal size="custom" id="modal-reg" :hide-footer="true" :hide-header="true" ref="modal-reg">
        <div class="container-fluid">
          <div class="row">
            <div class="col-12">
              <div class="reg-zag text-center">
                <h2>Регистрация</h2>
              </div>
              <div class="mt-3 mb-n2" v-if="danger_alert_show">
                <b-alert show variant="danger">{{ danger_alert }}</b-alert>
              </div>
              <b-form @submit="onSubmit">
                <b-form-group id="input-group-reg1" label="Ваше имя:" label-for="input-reg1">
                  <b-form-input
                    id="input-reg1"
                    v-model="form.name"
                    required
                    placeholder="Введите имя"
                  ></b-form-input>
                </b-form-group>
                <b-form-group
                  id="input-group-reg2"
                  label="Email адрес:"
                  label-for="input-reg2"
                  description="Мы никогда не передадим вашу электронную почту кому-либо еще."
                >
                  <b-form-input
                    id="input-reg2"
                    v-model="form.email"
                    type="email"
                    required
                    placeholder="Ваш email"
                  ></b-form-input>
                </b-form-group>

                <b-form-group id="input-group-reg3">
                  <label for="text-password">Пароль</label>
                  <b-input
                    required
                    type="password"
                    v-model="form.password"
                    id="reg-text-password"
                    aria-describedby="password-help-block"
                    placeholder="Пароль"
                  ></b-input>
                  <b-form-text id="reg-password-help-block">
                    Ваш пароль должен быть длиной 8-20 символов, содержать буквы и цифры и не должен
                    содержит пробелы, специальные символы или эмодзи.
                  </b-form-text>
                </b-form-group>
                <b-form-group id="input-group-reg4">
                  <label for="text-password">Повторите пароль</label>
                  <b-input
                    required
                    type="password"
                    v-model="form.сonfirm_password"
                    id="reg-text-сonfirm_password"
                    aria-describedby="сonfirm_password-help-block"
                    placeholder="Повторите пароль"
                  ></b-input>
                </b-form-group>
                <div class="reg-mod-button text-center">
                  <b-button type="submit" variant="primary">Зарегестрироваться</b-button>
                </div>
              </b-form>
            </div>
          </div>
        </div>
      </b-modal>

      <b-alert
        v-model="success_notification"
        class="position-fixed fixed-top m-0 rounded-0"
        style="z-index: 2000;"
        variant="success"
        dismissible
      >
        Для подтверждения вашего аккаунта сначала просмотрите папку входящих сообщений на почте, которую вы использовали при регистрации, чтобы проверить получили ли вы электронное письмо с подтверждением.<br/>
        <br />Если вы не получили данное письмо, проверьте папки «Спам» и «Удаленные», так как письмо могло автоматически туда перейти.<br/>
        <br />Если вы все еще не можете найти письмо для подтверждения вашего аккаунта (после того как убедитесь, что оно не находится в папке “спам”), или у вас возникли трудности в процессе регистрации вашего аккаунта, пожалуйста, отправьте запрос в нашу службу поддержки.
      </b-alert>
    </div>
  </div>
</template>

<script>
export default {
  name: "Mregistration",
  data() {
    return {
      form: {
        email: "",
        name: "",
        password: "",
        сonfirm_password: "",
        sitename: "Freelance manager",
        siteurl: "http://freelancemanager.ru/",
      },
      danger_alert: "",
      danger_alert_show: false,
      success_notification: false,
    };
  },
  methods: {
    onSubmit(evt) {
      evt.preventDefault();
      let self = this;
      if (this.form.password != "") {
        if (this.form.password == this.form.сonfirm_password) {
          this.$axios
            .post(this.$store.state.auth_host + "registration/save/", this.form)
            .then((response) => {
              if (response.data.status == "user_created") {
                this.success_notification = true;
                this.$refs["modal-reg"].hide();
                this.danger_alert_show = false;
                this.form.email = "";
                this.form.name = "";
                this.form.password = "";
                this.form.сonfirm_password = "";
              }
              if (response.data.status == "error") {
                this.danger_alert = response.data.description;
                this.danger_alert_show = true;
              }
            });
        } else {
          this.danger_alert = "Пароли должны совпадать";
          this.danger_alert_show = true;
          this.form.password = "";
          this.form.сonfirm_password = "";
        }
      }
    },
  },
};
</script>

<style>
.reg-mod-button .btn {
  background: #fff;
  color: black;
  border: 0.5px solid black;
  margin-top: 4px;
}

#modal-reg #modal-reg___BV_modal_body_ {
  padding: 17px 17px 24px 17px;
}

.reg-mod-button button:hover {
  background: #52d3aa;
  color: #fff;
  border: none;
  border-color: white;
  box-shadow: none;
}

.reg-mod-button button:focus {
  box-shadow: none;
}

#input-group-reg4 {
  margin-bottom: 23px;
}
</style>