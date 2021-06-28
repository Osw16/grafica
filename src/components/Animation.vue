<template>
  <div>
    <div class="container">
      <div class="stats-container">
        <div class="info-container">
          <h2>KDA</h2>
          <h2>CS/MIN</h2>
          <h2>GOLD/MIN</h2>
          <h2>DMG/MIN</h2>
        </div>
        <div class="progress-bar">
          <b-progress :max="kdaMax" class="mb-3">
            <b-progress-bar
              variant="success"
              :value="playerKda"
            ></b-progress-bar>
          </b-progress>
          <b-progress :max="creepsPerMinuteMax" class="mb-3">
            <b-progress-bar
              variant="success"
              :value="playerCreepsPerMinute"
            ></b-progress-bar>
          </b-progress>
          <b-progress :max="goldPerMinuteMax" class="mb-3">
            <b-progress-bar
              variant="success"
              :value="playerGoldPerMinute"
            ></b-progress-bar>
          </b-progress>
          <b-progress :max="damagePerMinuteMax" class="mb-3">
            <b-progress-bar
              variant="success"
              :value="playerDamagePerMinute"
            ></b-progress-bar>
          </b-progress>
        </div>

        <div class="max-container">
          <h1>{{ playerKda }}</h1>
          <h1>{{ playerCreepsPerMinute }}</h1>
          <h1>{{ playerGoldPerMinute }}</h1>
          <h1>{{ playerDamagePerMinute }}</h1>
        </div>
      </div>

      <div class="logo">
        <img :src="logoImagen" alt="logo-team" width="40" height="40" />
      </div>
      <div class="player-img">
        <img :src="playerImagen" alt="player" width="250" height="350" />
      </div>

      <div class="name-cont">
        <h1>{{ nick }}</h1>
      </div>
      <div class="champion-cont">
        <h1>Campeón más Usado</h1>
      </div>

      <div class="hero-img">
        <img alt="hero" src="../assets/ornn-hero.jpg" />
      </div>

      <div class="background">
        <img alt="grafica" src="../assets/ingame-lower.png" />
      </div>
    </div>
  </div>
</template>

<script>
import gsap from "gsap";
export default {
  data() {
    return {
      info: [],
      logoImagen: "",
      nick: "",
      playerImagen: "",
      goldPerMinute: "",
      damagePerMinute: "",
      creepsPerMinute: "",
      kdaMax: "",
      creepsPerMinuteMax: "",
      goldPerMinuteMax: "",
      damagePerMinuteMax: "",
      playerKda: "",
      playerCreepsPerMinute: "",
      playerGoldPerMinute: "",
      playerDamagePerMinute: "",
    };
  },
  mounted() {
    // animation
    
    gsap.to(".player-img", { y: 30, duration: 1,repeat:1, yoyo:true,repeatDelay:1, ease:"none"});
    gsap.to(".stats-container", { x: 30, duration: 1,repeat:1, yoyo:true,repeatDelay:1, ease:"none"});

    gsap.to(".container", {
      y: -225,
      duration: 1,
      repeat: 1,
      yoyo: true,
      repeatDelay: 10,
      ease: "none",
    });
    // fullscreen
    // gsap.to(".container", { y: -130, duration: 1,repeat:1, yoyo:true,repeatDelay:10, ease:"none"});
  },
  created() {
    // métrica maximas de la competicion
    fetch(
      "https://streaming.lvp.global/api/stats?url=https://api.lvp.global/lol/v1/competitions/a3141008-3e14-11eb-892c-065e1d3d7cd4/players"
    )
      .then((response) => response.json())
      .then((json) => {
        let data = json.data;
        // maximo valor de KDA
        let arrKda = data.map((m) => {
          return { kda: m.metrics.kda };
        });
        const mappedKda = arrKda.map((obj) => obj.kda);
        const maxKda = Math.max(...mappedKda);
        this.kdaMax = maxKda;
        // maximo valor de creepsPerMinute
        let arrCreepsXmin = data.map((m) => {
          return { creepsPerMinute: m.metrics.creepsPerMinute };
        });
        const mappedCreepsXmin = arrCreepsXmin.map(
          (obj) => obj.creepsPerMinute
        );
        const maxCreepsXmin = Math.max(...mappedCreepsXmin);
        this.creepsPerMinuteMax = maxCreepsXmin;
        // maximo valor de goldPerMinute
        let arrGoldXmin = data.map((m) => {
          return { goldPerMinute: m.metrics.goldPerMinute };
        });
        const mappedGoldXmin = arrGoldXmin.map((obj) => obj.goldPerMinute);
        const maxGoldXmin = Math.max(...mappedGoldXmin);
        this.goldPerMinuteMax = maxGoldXmin;
        // maximo valor de damagePerMinute:
        let arrDamageXmin = data.map((m) => {
          return { damagePerMinute: m.metrics.damagePerMinute };
        });
        const mappedDamageXmin = arrDamageXmin.map(
          (obj) => obj.damagePerMinute
        );
        const maxDamageXmin = Math.max(...mappedDamageXmin);
        this.damagePerMinuteMax = maxDamageXmin;
      });
      // datos del jugador
    fetch(
      "https://streaming.lvp.global/api/stats?url=https://api.lvp.global/lol/v1/competitions/a3141008-3e14-11eb-892c-065e1d3d7cd4/players/0b7c856e-15e0-11e9-88c3-22000ac39d70"
    )
      .then((response) => response.json())
      .then((info) => {
        const nickName = info.data.player.nickname;
        this.nick = nickName;
        this.playerImagen = info.data.player.photo.original;
        this.logoImagen = info.data.team.logo.original;
        const playerArrData = [
          (this.playerKda = info.data.metrics.kda),
          (this.playerCreepsPerMinute = info.data.metrics.creepsPerMinute),
          (this.playerGoldPerMinute = info.data.metrics.goldPerMinute),
          (this.playerDamagePerMinute = info.data.metrics.damagePerMinute),
        ];
        console.log(playerArrData);
      });

    // métricas del jugador
    fetch(
      "https://streaming.lvp.global/api/stats?url=https://api.lvp.global/lol/v1/competitions/a3141008-3e14-11eb-892c-065e1d3d7cd4/players/0b7c856e-15e0-11e9-88c3-22000ac39d70"
    )
      .then((response) => response.json())
      .then((info) => {
        this.playerImagen = info.data.player.photo.original;
        this.logoImagen = info.data.team.logo.original;
        this.kda = info.data.metrics.kda;
        this.creepsPerMinute = info.data.metrics.creepsPerMinute;
        this.goldPerMinute = info.data.metrics.goldPerMinute;
        this.damagePerMinute = info.data.metrics.damagePerMinute;
        this.info = info.data;
      });
  },
};
</script>

<style scoped>
img {
  max-height: 100%;
  display: block;
}
.container {
  margin-top: 200px;
  height: 700px;
}
.background {
  display: block;
  margin-left: auto;
  margin-right: auto;
  height: 697px;
}
.name-cont {
  position: absolute;
  top: 507px;
  left: 220px;
  width: 239px;
  height: 100px;
  display: grid;
  align-items: center;
  justify-content: center;
}
.champion-cont {
  position: absolute;
  top:542px;
  left: 870px;
  display: grid;
  align-items: center;
  justify-content: center;
}
.champion-cont h1 {
  font-size: 17px;
}
.max-container{
margin-top: -10px;
}
.max-container h1{
  margin: 6px;
  font-size: 20px;
}
.info-container h1{
  border: 1px solid white;
  margin: 6px;
  font-size: 20px;
}
.logo {
  top: 501px;
  left: 200px;
  position: absolute;
  width: 100px;
  height: 100px;
  display: grid;
  align-items: center;
  justify-content: center;
}
.player-img {
  z-index: 3;
  position: absolute;
  top: 515px;
  left: 693px;
  display: grid;
  align-items: center;
  justify-content: center;
}
.hero-img {
  z-index: 1;
  position: absolute;
  top: 570px;
  left: 839px;
  height: 129px;
}
.progress-bar {
  width: 360px;
  background-color: transparent;
  padding-right: 5px;
  padding-left: 5px;
}
.stats-container {
  position: absolute;
  top: 442px;
  left: 225px;
  width: 600px;
  height: 400px;
  display: flex;
  align-items: center;
}
h2 {
  font-size: 15px;
  justify-items: end;
  padding: 2.5px;
}
h1 {
  font-size: 25px;
}
</style>