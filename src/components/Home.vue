<template>
  <div class="container">
    <div hidden class="voice">
      <vue-speech lang="es-US" @onTranscriptionEnd="onEnd" />
    </div>
    <div class="title">
      <h1>Pomodoro &#127813;</h1>
    </div>
    <div class="container-btn-type">
      <button v-if="showTypeButtonPomodoro" class="btn-pomodoro-a"> Pomodoro</button>
      <button @click="handleButtonPomodoro" v-if="!showTypeButtonPomodoro" class="btn-pomodoro-b"> Pomodoro</button>
      <button v-if="showTypeButtonShortBreak" class="btn-shortBreak-a"> Descanso Corto</button>
      <button @click="handleButtonShortBreak" v-if="!showTypeButtonShortBreak" class="btn-shortBreak-b"> Descanso Corto</button>
      <button v-if="showTypeButtonLongBreak" class="btn-longBreak-a"> Descanso Largo</button>
      <button @click="handleButtonLongBreak" v-if="!showTypeButtonLongBreak" class="btn-longBreak-b"> Descanso Largo</button>
    </div>
    <div class="timer">
      <svg
        width="163"
        height="165"
        viewBox="-8 -8 184 188"
        fill="none"
        id="first-segment"
      >
        <path
          stroke="#E9D2B1"
          stroke-width="15"
          stroke-linecap="round"
          d="M165.16,163.38A172,172,0,0,0,0,0"
        />
        <path
          id="top-right"
          stroke="#F85959"
          stroke-width="15"
          stroke-linecap="round"
          d="M165.16,163.38A172,172,0,0,0,0,0"
        />
      </svg>
      <svg
        width="163"
        height="165"
        viewBox="-8 -8 184 188"
        fill="none"
        id="second-segment"
      >
        <path
          stroke="#E9D2B1"
          stroke-width="15"
          stroke-linecap="round"
          d="M0,163.34A172,172,0,0,0,164.44,0"
        />
        <path
          id="bottom-right"
          stroke="#F85959"
          stroke-width="15"
          stroke-linecap="round"
          d="M0,163.34A172,172,0,0,0,164.44,0"
        />
      </svg>
      <svg
        width="163"
        height="165"
        viewBox="-8 -8 184 188"
        fill="none"
        id="third-segment"
      >
        <path
          stroke="#E9D2B1"
          stroke-width="15"
          stroke-linecap="round"
          d="M0,0A172,172,0,0,0,165.16,162.61"
        />
        <path
          id="bottom-left"
          stroke="#F85959"
          stroke-width="15"
          stroke-linecap="round"
          d="M0,0A172,172,0,0,0,165.16,162.61"
        />
      </svg>
      <svg
        width="163"
        height="165"
        viewBox="-8 -8 184 188"
        fill="none"
        id="fourth-segment"
      >
        <path
          stroke="#E9D2B1"
          stroke-width="15"
          stroke-linecap="round"
          d="M160.17,0A172,172,0,0,0,0,161.51"
        />
        <path
          id="top-left"
          stroke="#F85959"
          stroke-width="15"
          stroke-linecap="round"
          d="M160.17,0A172,172,0,0,0,0,161.51"
        />
      </svg>
      <div class="time-display">
        <p v-if="resting">Descanso</p>
        <h2 class="number-time">{{ timeDisplay }}</h2>
      </div>
    </div>
    <div class="container-btn-star-end">
      <button class="btn-start-pause" @click="handleTimer" :disabled="resting">{{ buttonStarPause }}</button>
      <button class="btn-stop-done" @click="handleStoper" :disabled="resting">{{ buttonStopDone }}</button>
    </div>
    <div class="container-btn-right">
        <div class="container-buttons">
          <button v-b-toggle.sidebar  @click="showSidebarMain='sdConfClock'" class="btn-ctn-icon"><font-awesome-icon class="btn-icon" icon="clock"/></button>
          <button v-b-toggle.sidebar  @click=isLogin() class="btn-ctn-icon"><font-awesome-icon class="btn-icon" icon="user"/></button>
          <button v-b-toggle.sidebar @click="showSidebarMain='sdInfo'" class="btn-ctn-icon"><font-awesome-icon class="btn-icon" icon="info-circle"/></button>
          <button v-b-toggle.sidebar @click=salir() class="btn-ctn-icon"> <font-awesome-icon class="btn-icon" icon="sign-out-alt"/></button>
        </div>
      </div>
  <b-sidebar id="sidebar" class="sidebar sd-clock"
             title="Configurar tiempo" width="20em" right shadow backdrop>
    <div v-if="showSidebarMain == 'sdConfClock'" class="container-sidebar">
      <form @submit.prevent="configuracionTiempo">
        <p>Tiempo (minutos) </p>
        <p>Pomodoro</p>
        <input type="number" v-model="tiempoDePomodoro" value="tiempoDePomodoro">
        <p>Descanso Corto</p>
        <input type="number" v-model="tiempoDescansoCorto" value="tiempoDescansoCorto">
        <p>Descanso Largo</p>
        <input type="number" v-model="tiempoDescansoLargo" value="tiempoDescansoLargo">
        <button type="submit" class="btn-confg-time"> Aceptar </button>
      </form>
      <div class="container-btn-left">
        <div class="container-buttons">
          <button @click="showSidebarMain='sdConfClock'" class="btn-ctn-icon"><font-awesome-icon class="btn-icon" icon="clock"/></button>
          <button @click=isLogin() class="btn-ctn-icon"><font-awesome-icon class="btn-icon" icon="user"/></button>
          <button @click="showSidebarMain='sdInfo'" class="btn-ctn-icon"><font-awesome-icon class="btn-icon" icon="info-circle"/></button>
          <button @click=salir() class="btn-ctn-icon"> <font-awesome-icon class="btn-icon" icon="sign-out-alt"/></button>
        </div>
      </div>
    </div>
    <div v-if="showSidebarMain == 'sdLoginSignup'" class="container-sidebar">
      <div  class="bnt-login-signup">
        <button @click="showSidebarMain='SideFormularioRegistro'"> Registraste </button>
        <button @click="showSidebarMain='SideFormLogin'" > Ingresar </button>
      </div>
      <div class="container-btn-left">
        <div class="container-buttons">
          <button @click="showSidebarMain='sdConfClock'" class="btn-ctn-icon"><font-awesome-icon class="btn-icon" icon="clock"/></button>
          <button @click=isLogin() class="btn-ctn-icon"><font-awesome-icon class="btn-icon" icon="user"/></button>
          <button @click="showSidebarMain='sdInfo'" class="btn-ctn-icon"><font-awesome-icon class="btn-icon" icon="info-circle"/></button>
          <button @click=salir() class="btn-ctn-icon"> <font-awesome-icon class="btn-icon" icon="sign-out-alt"/></button>
        </div>
      </div>
    </div>
    <div v-if="showSidebarMain == 'sdInfo'" class="container-sidebar">
      <div  class="sd-container-info">
        <p>>Hola</p>
        <p>Bienvenido a pomodoro</p>
        <p>Aquí encontrará una guía sencilla para usar pomodoro.</p>
        <p>Elija una tarea que le gustaría realizar. Inicie el temporizador durante 25 minutos (25 minutos es la técnica Pomodoro tradicional, 
          pero puede configurar el temporizador para lo que funcione para usted). </p>
        <p>Trabaja enfocado en esa tarea hasta que suene el reloj.  Después de eso, tómate un breve descanso. </p>
        <p>Después de 4 pomodoros recomendamos un descanso más largo.</p>
        <p>Repita hasta que haya terminado. </p>
        <p>¡Estupendo! </p>
        <p>¡Eso es todo lo que necesita saber para</p>
        <p>realizar actividades más productivas!</p>
      </div>
      <div class="container-btn-left">
        <div class="container-buttons">
          <button @click="showSidebarMain='sdConfClock'" class="btn-ctn-icon"><font-awesome-icon class="btn-icon" icon="clock"/></button>
          <button @click=isLogin() class="btn-ctn-icon"><font-awesome-icon class="btn-icon" icon="user"/></button>
          <button @click="showSidebarMain='sdInfo'" class="btn-ctn-icon"><font-awesome-icon class="btn-icon" icon="info-circle"/></button>
          <button @click=salir() class="btn-ctn-icon"> <font-awesome-icon class="btn-icon" icon="sign-out-alt"/></button>
        </div>
      </div>
    </div>
    <div v-if="showSidebarMain == 'sdLogout'" class="container-sidebar">
      <div  class="bnt-login-signup">
        <p class="p-logout">Todavia no ha iniciado sesión</p>
        <button @click="showSidebarMain='SideFormularioRegistro'"> Registraste </button>
        <button @click="showSidebarMain='SideFormLogin'" > Ingresar </button>
      </div>
      <div class="container-btn-left">
        <div class="container-buttons">
          <button @click="showSidebarMain='sdConfClock'" class="btn-ctn-icon"><font-awesome-icon class="btn-icon" icon="clock"/></button>
          <button @click=isLogin() class="btn-ctn-icon"><font-awesome-icon class="btn-icon" icon="user"/></button>
          <button @click="showSidebarMain='sdInfo'" class="btn-ctn-icon"><font-awesome-icon class="btn-icon" icon="info-circle"/></button>
          <button @click=salir() class="btn-ctn-icon"> <font-awesome-icon class="btn-icon" icon="sign-out-alt"/></button>
        </div>
      </div>
    </div>
    <div v-if="showSidebarMain == 'salirSesion'" class="container-sidebar">
      <div  class="bnt-login-signup">
        <p class="p-logout">¿Desear cerrar sesión?</p>
        <button @click=SiSalir()> SI </button>
        <button @click="showSidebarMain='PerfilUsuario'" > NO </button>
      </div>
      <div class="container-btn-left">
        <div class="container-buttons">
          <button @click="showSidebarMain='sdConfClock'" class="btn-ctn-icon"><font-awesome-icon class="btn-icon" icon="clock"/></button>
          <button @click=isLogin() class="btn-ctn-icon"><font-awesome-icon class="btn-icon" icon="user"/></button>
          <button @click="showSidebarMain='sdInfo'" class="btn-ctn-icon"><font-awesome-icon class="btn-icon" icon="info-circle"/></button>
          <button @click=salir() class="btn-ctn-icon"> <font-awesome-icon class="btn-icon" icon="sign-out-alt"/></button>
        </div>
    </div>
    </div>
    <div v-if="showSidebarMain == 'SideFormularioRegistro'" class="container-sidebar">
      <form @submit.prevent="registrarUsuario">
        <p>Nombre de usuario</p>
        <input type="text" v-model="user.username" value="">
        <p>Correo electronico</p>
        <input type="email" v-model="user.email" value="">
        <p>Constraseña</p>
        <input type="password" v-model="user.password" value="">
        <p>Confirmación</p>
        <input type="password" v-model="confirmPassword" value="">
        <button type="submit" class="btn-confg-time"> Registraste </button>
      </form>
      <div class="container-btn-left">
        <div class="container-buttons">
          <button @click="showSidebarMain='sdConfClock'" class="btn-ctn-icon"><font-awesome-icon class="btn-icon" icon="clock"/></button>
          <button @click=isLogin() class="btn-ctn-icon"><font-awesome-icon class="btn-icon" icon="user"/></button>
          <button @click="showSidebarMain='sdInfo'" class="btn-ctn-icon"><font-awesome-icon class="btn-icon" icon="info-circle"/></button>
          <button @click=salir() class="btn-ctn-icon"> <font-awesome-icon class="btn-icon" icon="sign-out-alt"/></button>
        </div>
      </div>
    </div>
    <div v-if="showSidebarMain == 'SideFormLogin'" class="container-sidebar">
      <form @submit.prevent="loginUsuario">
        <p>Email</p>
        <input type="email" v-model="user.email" value="">
        <p>Contraseña</p>
        <input type="password" v-model="user.password" value="">
        <button type="submit" class="btn-confg-time"> Ingresar </button>
      </form>
      <div class="container-btn-left">
        <div class="container-buttons">
          <button @click="showSidebarMain='sdConfClock'" class="btn-ctn-icon"><font-awesome-icon class="btn-icon" icon="clock"/></button>
          <button @click=isLogin() class="btn-ctn-icon"><font-awesome-icon class="btn-icon" icon="user"/></button>
          <button @click="showSidebarMain='sdInfo'" class="btn-ctn-icon"><font-awesome-icon class="btn-icon" icon="info-circle"/></button>
          <button @click=salir() class="btn-ctn-icon"> <font-awesome-icon class="btn-icon" icon="sign-out-alt"/></button>
        </div>
      </div>
    </div>
    <div v-if="showSidebarMain == 'PerfilUsuario'" class="container-sidebar">
      <div class="container-perfil">
        <img class="img-perfil" src="../assets/Perfil.png" />
        <p class="p-perfil-username"> {{user.username}} </p>
        <p class="p-perfil-email"> {{user.email}} </p>
      </div>
      <div class="continer-tiempo-hecho">
        <p>Pomodoros del dia</p>
        <input type="text" value="1" disabled>
        <p>Pomodoros de la semana</p>
        <input type="text" value="1" disabled>
        <p>Pomodoros del mes</p>
        <input type="text" value="1" disabled>
      </div>
<!--       <form @submit.prevent="agregarTarea">
        <p>Tarea</p>
        <input type="text" v-model="user.newTask" value="">
        <button type="submit" class="btn-agregar-tarea"> Agregar Tarea </button>
      </form> -->
      <div class="container-btn-left">
        <div class="container-buttons">
          <button @click="showSidebarMain='sdConfClock'" class="btn-ctn-icon"><font-awesome-icon class="btn-icon" icon="clock"/></button>
          <button @click=isLogin() class="btn-ctn-icon"><font-awesome-icon class="btn-icon" icon="user"/></button>
          <button @click="showSidebarMain='sdInfo'" class="btn-ctn-icon"><font-awesome-icon class="btn-icon" icon="info-circle"/></button>
          <button @click=salir() class="btn-ctn-icon"> <font-awesome-icon class="btn-icon" icon="sign-out-alt"/></button>
        </div>
      </div>
    </div>
  </b-sidebar>
  </div>
</template>

<script>
import firebase from "firebase";
import bcryptjs from "bcryptjs"
import ProgressBar from "progressbar.js";
import beep from "../assets/beep.mp3";
import confetti from "canvas-confetti";
export default {
  created(){
    //console.log("Created")
  },
  name: "Home",
  data: () => {
    var pomodoroDuration = 10;
    return {
      session: false,
      user: {
        username:'',
        email: '',
        password: '',
        pomodoro: '25',
        breakShor: '5',
        breakLong: '15',
        // newTask: '',
      },
      confirmPassword: '',
      tiempoDescansoLargo: 15,
      tiempoDescansoCorto: 5,
      tiempoDePomodoro: 25,
      showSidebarMain: '',
      restLargoDuration:5,
      restCortoDuration:3,
      pomodoroDuration:10,
      restDuration: 3,
      currentTimeInSeconds: pomodoroDuration,
      currentSegment: 1,
      buttonStarPause: "¡Iniciar!",
      buttonStopDone: "¡Parar!",
      showTypeButtonPomodoro: true,
      showTypeButtonLongBreak: false,
      showTypeButtonShortBreak: false,
      topRight: null,
      bottomRight: null,
      bottomLeft: null,
      topLeft: null,
      pathOptions: {
        easing: "linear",
        duration: (pomodoroDuration + 1) * 1000, // add a sec and convert to millis
      },
      interval: null,
      timeout: null,
      beepAudio: new Audio(beep),
      resting: false,
    };
  },
  mounted: function() {
    this.topRight = new ProgressBar.Path("#top-right", this.pathOptions);
    this.topRight.set(1);
    this.bottomRight = new ProgressBar.Path("#bottom-right", this.pathOptions);
    this.bottomRight.set(1);
    this.bottomLeft = new ProgressBar.Path("#bottom-left", this.pathOptions);
    this.bottomLeft.set(1);
    this.topLeft = new ProgressBar.Path("#top-left", this.pathOptions);
    this.topLeft.set(1);
  },
  methods: {
    SiSalir(){
      this.session = false;
      this.showSidebarMain = 'sdLoginSignup';
      this.user.email = '';
      this.user.password = '';
    },
    salir(){
      if (!this.session){
        this.showSidebarMain = 'sdLogout';
      }
      if (this.session){
        this.showSidebarMain = 'salirSesion';
      }
    },
    isLogin(){
      if (this.session){
        this.showSidebarMain = 'PerfilUsuario';
      }
      if (!this.session){
        this.showSidebarMain = 'sdLoginSignup';
      }
    },
    pruebaCargarDatosFirebase(){
      firebase.database().ref('/usuarios').set({name: "Usuario1"})
    },
    agregarTarea() {
      console.log(this.user.newTask)
    },
    traerDatos(){
      var ref = firebase.database().ref("/usuarios");
      ref.orderByChild("email").equalTo(this.user.email).on("child_added", snapshot => this.manejoData(snapshot.val()))

    },
    manejoData(data){
      if (bcryptjs.compareSync(this.user.password, data.password)){
          this.user.username = data.username;
          this.user.pomodoro = data.pomodoro;
          this.user.breakShor = data.tiempoDescansoCorto;
          this.user.breakLong = data.tiempoDescansoLargo;
          this.session = true;
          this.showSidebarMain = 'PerfilUsuario';
      }
      else {
        console.log("Contraseña incorrecta")
      }
    },
    loginUsuario() {
      this.traerDatos();
      console.log("Sigue despues de traer datos");
    },
    registrarUsuario(){
      var salt = bcryptjs.genSaltSync(10);
      var hasheduser = {
        username: this.user.username,
        email: this.user.email,
        password: bcryptjs.hashSync(this.user.password, salt),
        tiempoDescansoLargo: this.user.pomodoro,
        tiempoDescansoCorto: this.user.breakShor,
        tiempoDePomodoro: this.user.breakLong,
      };
      console.log(hasheduser)
      firebase.database().ref('/usuarios').push().set(hasheduser)
      this.session = true;
      this.showSidebarMain = 'PerfilUsuario'
    },
    configuracionTiempo(){
      this.restLargoDuration=this.tiempoDescansoLargo*60;
      this.user.pomodoro = this.tiempoDescansoLargo*60;
      this.restCortoDuration = this.tiempoDescansoCorto*60;
      this.user.breakShor = this.tiempoDescansoCorto*60;
      this.pomodoroDuration = this.tiempoDePomodoro*60;
      this.user.breakLong = this.tiempoDescansoLargo*60;
      this.pathOptions["duration"]=(parseInt(this.pomodoroDuration)+1)*1000;
      this.currentTimeInSeconds = this.pomodoroDuration;
    },
    handleButtonPomodoro() {
      clearInterval(this.interval);
      clearTimeout(this.timeout);
      this.buttonStarPause = "¡Iniciar!";
      this.resting = false;
      this.handleStoper();
      this.showTypeButtonPomodoro = true;
      this.showTypeButtonShortBreak = false;
      this.showTypeButtonLongBreak = false;
    },
    handleButtonShortBreak() {
      this.handleStoper();
      this.restDuration = this.restCortoDuration,
      this.buttonRest();
      this.showTypeButtonPomodoro = false;
      this.showTypeButtonShortBreak = true;
      this.showTypeButtonLongBreak = false;
    },
    handleButtonLongBreak() {
      this.handleStoper();
      this.restDuration = this.restLargoDuration,
      this.buttonRest();
      this.showTypeButtonPomodoro = false;
      this.showTypeButtonShortBreak = false;
      this.showTypeButtonLongBreak = true;
    },
    handleTimer() {
      if (this.buttonStarPause === "¡Iniciar!" || this.buttonStarPause === "Continuar") {
        this.animateBar();
        this.buttonStarPause = "Pausa";
      } else if (this.buttonStarPause === "Pausa") {
        this.pauseBar();
        this.buttonStarPause = "Continuar";
      }
    },
    handleStoper() {
      this.currentSegment = 1;
      this.currentTimeInSeconds= this.pomodoroDuration;
      this.topRight.set(1);
      this.topLeft.set(1);
      this.bottomRight.set(1);
      this.bottomLeft.set(1);
      this.buttonStarPause = "¡Iniciar!"
      this.pauseBar();
    },
    onEnd ({ lastSentence}) {
      console.log(lastSentence);
      if(lastSentence.includes("pomodoro iniciar") && this.buttonStarPause === "¡Iniciar!"){
        this.handleTimer()
      }
      if(lastSentence.includes("pomodoro continuar") && this.buttonStarPause === "Continuar"){
        this.handleTimer()
      }
      if(lastSentence.includes("pomodoro pausa") && this.buttonStarPause === "Pausa"){
        this.handleTimer()
      }
      if(lastSentence.includes("pomodoro pomodoro")){
        this.handleButtonPomodoro()
      }
      if(lastSentence.includes("pomodoro parar")){
        this.handleStoper()
      }
      if(lastSentence.includes("pomodoro descanso corto")){
        this.handleButtonShortBreak()
      }
      if(lastSentence.includes("pomodoro descanso largo")){
        this.handleButtonLongBreak()
      }
    },
    animateBar() {
      this.reduceTime();
      let segment = null;
      switch (this.currentSegment) {
        case 1:
          segment = this.topRight;
          break;
        case 2:
          segment = this.bottomRight;
          break;
        case 3:
          segment = this.bottomLeft;
          break;
        case 4:
          segment = this.topLeft;
          break;
      }
      segment.animate(
        0,
        {
          duration: (parseInt(this.currentTimeInSeconds) + 1) * 1000,
        },
        this.onFinish
      );
    },
    pauseBar() {
      clearInterval(this.interval);
      switch (this.currentSegment) {
        case 1:
          this.topRight.stop();
          break;
        case 2:
          this.bottomRight.stop();
          break;
        case 3:
          this.bottomLeft.stop();
          break;
        case 4:
          this.topLeft.stop();
          break;
      }
    },
    buttonRest(){
        this.resting = true;
        this.buttonStarPause = "Descanso";
        setTimeout(() => {
          // Change time to reflect rest duration
          this.currentTimeInSeconds = this.restDuration;
          // Start rest after beep ends
          this.startRest();
        }, 0);
    },
    onFinish() {
      if (this.currentTimeInSeconds <= 0) {
        // When finish, we want it to beep for a few seconds then only start rest timer
        if (this.currentSegment < 4) {
          this.currentSegment += 1;
        } else {
          // Reset all
          this.topRight.set(1);
          this.topLeft.set(1);
          this.bottomRight.set(1);
          this.bottomLeft.set(1);
          confetti({
            particleCount: 300,
            spread: 100,
            origin: { y: 0.7 },
          });
          this.currentSegment = 1;
        }
        // Clear interval
        clearInterval(this.interval);
        // Play audio
        this.beepAudio.play();
        // Immediately disable button and set state
        this.resting = true;
        this.buttonStarPause = "Descanso";
        setTimeout(() => {
          // Change time to reflect rest duration
          this.currentTimeInSeconds = this.restDuration;
          // Start rest after beep ends
          this.startRest();
        }, 4200);
      }
    },
    reduceTime() {
      this.interval = setInterval(() => {
        if (this.currentTimeInSeconds > 0) {
          this.currentTimeInSeconds -= 1;
        }
      }, 1000);
    },
    startRest() {
      // Set new interval
      this.reduceTime();
      this.timeout = setTimeout(() => {
        clearInterval(this.interval);
        this.beepAudio.play();
        this.currentTimeInSeconds = this.pomodoroDuration;
        this.showTypeButtonPomodoro = true;
        this.showTypeButtonShortBreak = false;
        this.showTypeButtonLongBreak = false;
        this.buttonStarPause = "¡Iniciar!";
        this.resting = false;
      }, this.restDuration * 1000);
    },
  },
  computed: {
    timeDisplay() {
      const minutes = String(parseInt(this.currentTimeInSeconds / 60));
      const seconds = String(this.currentTimeInSeconds % 60);
      const paddedMinutes = ("0" + minutes).slice(-2);
      const paddedSeconds = ("0" + seconds).slice(-2);
      return `${paddedMinutes}:${paddedSeconds}`;
    },
  },
};
</script>

<style>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.title {
  margin-top: 50px;
}
h1 {
  color: #c44747;
  font-size: 3rem;
}
.timer {
  margin-top: 50px;
  width: 190px;
  height: 190px;
  position: relative;
  display: flex;
}
#first-segment {
  position: absolute;
  top: 0px;
  right: 0px;
  width: 6rem;
  height: 6rem;
}
#second-segment {
  position: absolute;
  bottom: 0px;
  right: 0px;
  width: 6rem;
  height: 6rem;
}
#third-segment {
  position: absolute;
  bottom: 0px;
  left: 0px;
  width: 6rem;
  height: 6rem;
}
#fourth-segment {
  position: absolute;
  top: 0px;
  left: 0px;
  width: 6rem;
  height: 6rem;
}
.time-display {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.time-display .number-time{
  font-size: 2.5rem;
}
.time-display p{
  font-size: 1.5rem;
}
p {
  font-size: 48px;
  line-height: 48px;
  text-align: center;
  color: #ff8080;
}
h2 {
  font-size: 64px;
  color: #f85959;
  text-align: center;
}
button:focus {
  outline: none;
}
button:disabled {
  opacity: 0.6;
  cursor: default;
}
canvas {
  background: none;
}
.container-btn-star-end{
  display: flex;
  justify-content: space-between;
  width: 15rem;
}
.container-btn-star-end .btn-start-pause{
  margin: 1rem 0 0 0;
  width: 7rem;
  height: 3.5rem;
  background: #f85959;
  border-radius: 20px;
  border: none;
  cursor: pointer;
  font-size: 1rem;
  line-height: 45px;
  text-align: center;
  color: #fff8ee;
}
.container-btn-star-end .btn-stop-done{
  margin: 1rem 0 0 0;
  width: 7rem;
  height: 3.5rem;
  background: #f85959;
  border-radius: 20px;
  border: none;
  cursor: pointer;
  font-size: 1rem;
  line-height: 45px;
  text-align: center;
  color: #fff8ee;
}
.container-btn-type{
  display: flex;
}
.container-btn-type .btn-pomodoro-a{
  margin: 1rem 0 0 0;
  width: 7rem;
  height: 3.5rem;
  background: #f85959;
  border-radius: 20px;
  border: none;
  cursor: pointer;
  font-size: 1rem;
  line-height: 45px;
  text-align: center;
  color: #fff8ee;
}
.container-btn-type .btn-shortBreak-a{
  margin: 1rem 0 0 0;
  width: 7rem;
  height: 3.5rem;
  background: #f85959;
  border-radius: 20px;
  border: none;
  cursor: pointer;
  font-size: 1rem;
  line-height: 45px;
  text-align: center;
  color: #fff8ee;
}
.container-btn-type .btn-longBreak-a{
  margin: 1rem 0 0 0;
  width: 7rem;
  height: 3.5rem;
  background: #f85959;
  border-radius: 20px;
  border: none;
  cursor: pointer;
  font-size: 1rem;
  line-height: 45px;
  text-align: center;
  color: #fff8ee;
}
.container-btn-type .btn-shortBreak-b{
  margin: 1rem 0 0 0;
  width: 7rem;
  height: 3.5rem;
  background: #fff8ee;
  border-radius: 20px;
  border: none;
  cursor: pointer;
  font-size: 1rem;
  line-height: 45px;
  text-align: center;
  color: #f85959;
}
.container-btn-type .btn-pomodoro-b{
  margin: 1rem 0 0 0;
  width: 7rem;
  height: 3.5rem;
  background: #fff8ee;
  border-radius: 20px;
  border: none;
  cursor: pointer;
  font-size: 1rem;
  line-height: 45px;
  text-align: center;
  color: #f85959;
}
.container-btn-type .btn-longBreak-b{
  margin: 1rem 0 0 0;
  width: 7rem;
  height: 3.5rem;
  background: #fff8ee;
  border-radius: 20px;
  border: none;
  cursor: pointer;
  font-size: 1rem;
  line-height: 45px;
  text-align: center;
  color: #f85959;
}
.container-btn-right{
  position: absolute;
  left: 100%; /* X */
  top: 50%; /* Y */
  transform: translate(-100%, -65%); /* Reposicionar X,Y */
}
.container-btn-right .container-buttons{
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  align-content: center;
}
.btn-icon{
  width: 100%;
  height: 100%;
}
.btn-ctn-icon{
  width: 50px;
  height: 50px;
  border: none;
  background-color: #fff8ee;
}
.sidebar{
  background: #fff8ee;
}
.b-sidebar-header{
  background-color: #fff8ee;
}
b-sidebar-body{
  background-color: #fff8ee;
}
.container-sidebar{
  background-color: #fff8ee;
  width: 100%;
  height: 100%;
}
.container-sidebar form{
  position: absolute;
  width: 80%;
  right: 0;
}
.container-sidebar form p{
  font-size: 1.5rem;
  text-align: start;
  line-height: 24px;
  margin-top: 10px;
}
.container-sidebar form input{
  outline: none;
  background-color: #FBE0BA;
  border-radius: 5%;
  border: 0;
  height: 2.5rem;
  text-indent: 10px;
  width: 85%;
}
/* FBE0BA */
.btn-confg-time{
  margin: 1.5rem 0 0 8.5rem;
  width: 5rem;
  height: 2.5rem;
  background: #f85959;
  border-radius: 20px;
  border: none;
  cursor: pointer;
  font-size: 1rem;
  line-height: 45px;
  text-align: center;
  color: #fff8ee;
}
.container-btn-left{
  position: absolute;
  left: 0; /* X */
  top: 50%; /* Y */
  transform: translate(0, -65%); /* Reposicionar X,Y */
}
.container-btn-left .container-buttons{
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  align-content: center;
}
.bnt-login-signup{
  position: absolute;
  width: 80%;
  height: 75%;
  right: 0;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.bnt-login-signup button{
  margin: 1rem 1rem 1rem 1rem;
  width: 5rem;
  height: 2.5rem;
  background: #f85959;
  border-radius: 20px;
  border: none;
  cursor: pointer;
  font-size: 1rem;
  line-height: 45px;
  text-align: center;
  color: #fff8ee;
}
.sd-container-info{
  position: absolute;
  width: 80%;
  height: 100%;
  right: 0;
}
.sd-container-info p{
  font-size: 1rem;
  text-align: start;
  line-height: 16px;
  margin-top: 10px;
}

.p-logout{
  font-size: 1rem;
}

.btn-agregar-tarea{
  margin: 1.5rem 0 0 4rem;
  width: 10rem;
  height: 2.5rem;
  background: #f85959;
  border-radius: 20px;
  border: none;
  cursor: pointer;
  font-size: 1rem;
  line-height: 45px;
  text-align: center;
  color: #fff8ee;
}

.container-perfil{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.img-perfil{
  width:140px;
  height:140px;
  border-radius:70px;
}

.p-perfil-username, .p-perfil-email{
  font-size: 1rem;
}


.container-sidebar .continer-tiempo-hecho{
  position: absolute;
  width: 80%;
  top: 300px;
  right: 0;
}


.continer-tiempo-hecho p{
  font-size: 1rem;
}

.continer-tiempo-hecho input{
  margin: 0 0 0 10px;
  outline: none;
  background-color: #FBE0BA;
  border-radius: 5%;
  border: 0;
  height: 2.5rem;
  text-indent: 10px;
  width: 80%;
}

</style>