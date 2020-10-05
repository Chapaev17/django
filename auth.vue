<template>
  <div class="auth">
    <div class="modal-au">
      <b-modal size="custom" id="modal-au" :hide-footer="true" :hide-header="true" ref="modal-au">
        <div class="container-fluid">
          <div class="row">
            <div class="col-12">
              <div class="au-zag text-center">
                <h2>Авторизация</h2>
              </div>
              <div class="mt-3 mb-n2" v-if="danger_alert_show">
                <b-alert show variant="danger">{{ danger_alert }}</b-alert>
              </div>
              <b-form @submit="onSubmit">
                <b-form-group id="input-group-au1" label="Логин:" label-for="input-au1">
                  <b-form-input
                    id="input-au2"
                    v-model="form.login"
                    required
                    placeholder="Ваш логин"
                  ></b-form-input>
                </b-form-group>
                <b-form-group id="input-group-au3">
                  <label for="au-text-password">Пароль</label>
                  <b-input
                    type="password"
                    v-model="form.password"
                    required
                    id="au-text-password"
                    aria-describedby="password-help-block"
                    placeholder="Пароль"
                  ></b-input>
                </b-form-group>
                <div class="au-mod-button text-center">
                  <b-button type="submit" variant="primary">Войти</b-button>
                </div>
              </b-form>
            </div>
          </div>
        </div>
      </b-modal>
    </div>
  </div>
</template>

<script>
export default {
  name: "Mauth",
  data() {
    return {
      form: {
        login: "",
        password: "",
      },
      danger_alert: "",
      danger_alert_show: false,
    };
  },
  methods: {
    async onSubmit(evt) {
      evt.preventDefault();
      try {
        let response = await this.$auth.loginWith("nlchat", {
          data: this.form,
        });
        this.$refs["modal-au"].hide();
        this.danger_alert_show = false;
        this.form.password = "";
      } catch (err) {
        if (err.response.data.status == "Login or password is incorrect") {
          this.danger_alert = "Неверный логин или пароль";
          this.danger_alert_show = true;
        }
        if (err.response.data.status == "User is not verified") {
          let mes =
            "Мы отправили Вам сообщение по электронной почте, чтобы " +
            "подтвердить Ваш адрес (проверьте папку спама и корзину, если Вы " +
            "не получили это сообщение). Вы сможете авторизоваться, когда " +
            "подтвердите Ваш адрес.";
          this.danger_alert = mes;
          this.danger_alert_show = true;
          this.form.password = "";
        }
      }
    },
  },
};
</script>

<style>
.au-mod-button .btn {
  background: #fff;
  color: black;
  border: 0.5px solid black;
  margin-top: 17px;
}

#modal-au #modal-au___BV_modal_body_ {
  padding: 17px 17px 24px 17px;
}

.au-zag {
  margin-bottom: 20px;
}

.au-mod-button button:hover {
  background: #52d3aa;
  color: #fff;
  border: none;
  border-color: white;
  box-shadow: none;
}

.au-mod-button button:focus {
  box-shadow: none;
}
</style>