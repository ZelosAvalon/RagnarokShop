<template lang="pug">
  .login-section.d-flex.justify-content-center.align-items-center.flex-column
    loading(:active.sync='isLoading', :opacity='.9')
      img(src='@/assets/loading.gif', alt='', srcset='')
      vue-typed-js.justify-content-center.align-items-center(:strings="['波利加載中…']")
        small.font-weight-normal.typing
    #login-logo.login-logo
    #login-bg.login-bg
      #login-top.login-top
      form(@submit.prevent='signin')
        input#email(type='email', required='', autofocus='', v-model='user.username')
        input#password(type='password', required='', v-model='user.password')
        button#musicPlay.btn-play(type='button', @click='musicStatus()')
          | stop
        button.btn-login(type='submit')
          | 登入
        router-link(to='/index')
          button.btn-end
            | 結束
    audio#roBGM(loop='', muted='', webkit-playsinline='true', playsinline='true')
      source(src='https://raw.githubusercontent.com/hsiangfeng/RagnarokShop/gh-pages/static/01.mp3', type='audio/mpeg')
</template>

<style lang="scss" scoped>
html,
body {
  height: 100%;
}
.form-signin {
  width: 100%;
  max-width: 330px;
  padding: 15px;
  margin: auto;
  background-color: rgba(0, 0, 0, 0.6);
}
.form-signin .checkbox {
  font-weight: 400;
}
.form-signin .form-control {
  position: relative;
  box-sizing: border-box;
  height: auto;
  padding: 10px;
  font-size: 16px;
}
.form-signin .form-control:focus {
  z-index: 2;
}
.form-signin input[type="email"] {
  margin-bottom: -1px;
  border-bottom-right-radius: 0;
  border-bottom-left-radius: 0;
}
.form-signin input[type="password"] {
  margin-bottom: 10px;
  border-top-left-radius: 0;
  border-top-right-radius: 0;
}
.login-section {
  height: 100vh;
  background-image: url(~@/assets/img/RTC_1920_1080.jpg);
  background-position: center top;
  position: relative;
  cursor: url(../assets/img/normal_select.png), auto;
}
.login-logo{
  position: absolute;
  top: 20%;
  left: 30%;
  background-image: url(../assets/img/logo.png);
  background-position: center center;
  background-repeat: no-repeat;
  width: 202px;
  height: 130px;
}
.login-bg{
  height: 120px;
  width: 280px;
  background-image: url(../assets/img/login.png);
  background-position: center center;
  background-repeat: no-repeat;
  border-radius: 5px;
  position: absolute;
  bottom: 25%;
  .login-top{
    position: absolute;
    width: 100%;
    height: 17px;
    z-index: 5;
  }
  #email{
    position: absolute;
    width: 130.5px;
    height: 19px;
    left: 32.4%;
    top: 24%;
    border: 0px;
    border: 1px solid #b9b8b8;
    font-size: 12px;
  }
  #password{
    position: absolute;
    width: 130.5px;
    height: 19px;
    left: 32.4%;
    top: 51%;
    border: 0px;
    border: 1px solid #b9b8b8;
    font-size: 12px;
  }
  .btn-login{
    position: absolute;
    bottom: 3%;
    right: 17.5%;
    width: 42px;
    font-size: 12px;
    height: 21px;
    border-radius: 5px;
    border: 0px;
    box-shadow: 0px 0px 2px #000;
    background-color: #eae8e8;
    font-weight: bold;
    cursor: url(../assets/img/link_select.png), auto;
  }
  .btn-end{
    position: absolute;
    bottom: 3%;
    right: 1.5%;
    width: 42px;
    font-size: 12px;
    height: 21px;
    border-radius: 5px;
    border: 0px;
    box-shadow: 0px 0px 2px #000;
    background-color: #eae8e8;
    font-weight: bold;
    cursor: url(../assets/img/link_select.png), auto;
  }
  .btn-play{
    position: absolute;
    font-size: 12px;
    bottom: 3.5%;
    left: 3%;
    width: 56px;
    height: 20px;
    border-radius: 3px;
    border: 0px;
    box-shadow: 0px 0px 2px #000;
    background-color: #eae8e8;
    cursor: url(../assets/img/link_select.png), auto;
  }
}
</style>

<script>
export default {
  data() {
    return {
      user: {
        username: '',
        password: '',
      },
      isLoading: false,
    };
  },
  methods: {
    signin() {
      const vm = this;
      const url = `${process.env.APIPATH}/admin/signin`;
      vm.isLoading = true;
      vm.$http.post(url, vm.user).then((response) => {
        if (response.data.success) {
          vm.isLoading = false;
          vm.$router.push('/admin');
        }
      });
    },
    dragWindow() {
      const loginLogoId = document.getElementById('login-logo');
      const loginTop = document.getElementById('login-top');
      const loginBg = document.getElementById('login-bg');
      function getPosition(event) {
        loginLogoId.style.left = `${event.clientX}px`;
        loginLogoId.style.top = `${event.clientY}px`;
      }
      function positionOver(event) {
        loginLogoId.style.left = `${event.clientX}px`;
        loginLogoId.style.top = `${event.clientY}px`;
      }
      function getPosition2(event) {
        loginBg.style.left = `${event.clientX}px`;
        loginBg.style.top = `${event.clientY}px`;
      }
      function positionOver2(event) {
        loginBg.style.left = `${event.clientX}px`;
        loginBg.style.top = `${event.clientY}px`;
      }
      loginLogoId.addEventListener('drag', getPosition, false);
      loginLogoId.addEventListener('dragend', positionOver, false);
      loginTop.addEventListener('drag', getPosition2, false);
      loginTop.addEventListener('dragend', positionOver2, false);
    },
    autoPlayMusic() {
      const roBGM = document.getElementById('roBGM');
      if (roBGM.paused) {
        roBGM.play();
        roBGM.volume = 0.2;
      }
    },
    musicStatus() {
      const musicPlay = document.getElementById('musicPlay');
      const roBGM = document.getElementById('roBGM');
      if (roBGM.paused) {
        roBGM.play();
        musicPlay.innerHTML = 'stop';
      } else {
        roBGM.pause();
        musicPlay.textContent = 'play';
      }
    },
  },
  mounted() {
    this.dragWindow();
    this.autoPlayMusic();
  },
};
</script>
