<template>
  <div id="main" class="text-white bg-primary" :class='{cloudyImage:data.temperature<20}'>
    <h1 class="text-center">Vue Weather Application</h1>
    <hr style="background:white" />
    <div class="container">
      <form class="mt-4" v-on:submit.prevent="searchLocation">
        <input
          type="text"
          class="form-control search-input"
          @change="apiError=false"
          placeholder="Search for a City... "
          v-model="searchCity"
          autocomplete="off"
        />
      </form>
      <span class="ml-2 error-text" v-show="apiError"
        >Search correct city name</span
      >
    <div class="text-center container mt-3 card card-main overflow-hidden" v-if='resultFound'>
      <div class="card-top">
        <h1 class="my-3 card-title">{{ data.city }}</h1>
        <h4>{{ data.country }}</h4>
        <h5></h5>
      </div>
      <div class="card-body my-3">
        <div class="line-height-min">
          <p>{{ data.desc }}</p>
          <span class="temp-text">{{ data.temperature }}&deg;C</span>
          <div class='row my-5'>
              <div class="col-6 text-primary">Lowest : {{data.low}}&deg;C</div>
              <div class="col-6 text-warning">Highest : {{data.high}}&deg;C</div>
          </div>
        </div>
      </div>
      <div class="row my-3 p-2">
        <div class='col-6 col-lg-12 line-height-min mb-5'>
          <p >Feeling like</p>
          <span class='bottom-text'>{{data.feel}}&deg;C</span>
        </div>
        <div class='col-6 col-lg-12 line-height-min'>
          <p >Humidity</p>
          <span class='bottom-text'>{{data.humid}}%</span>
        </div>
      </div>
    </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
  data() {
    return {
      searchCity: "",
      apiError: false,
      resultFound:false,
      key : '306e94b9ec539cb7a25815ad9a1cb2cf',
      baseUrl :'https://api.openweathermap.org/data/2.5/',
      data: {
        city: "",
        country: "",
        temperature: "",
        low: "",
        high: "",
        desc: "",
        humid: "",
        feel: "",
      },
    };
  },
  methods: {
    searchLocation: async function() {
      fetch(`${this.baseUrl}weather?q=${this.searchCity}&appid=${this.key}&units=metric`,
      ).then(res=>{
        return res.json() ;
        }).then(this.setResult) ;
    },
    setResult(result){
      if(result.cod == '404' || result.cod == '400'){
        this.apiError = true ;
        this.resultFound = false;
        }
      else{  
        this.resultFound = true ;
        this.apiError = false;
        this.data.city = result.name ;
        this.data.country = result.sys.country ;
        this.data.temperature = Math.round(result.main.temp) ;
        this.data.low = Math.round(result.main.temp_min) ;
        this.data.high = Math.round(result.main.temp_max) ;
        this.data.desc = result.weather[0].description ;
        this.data.humid = Math.round(result.main.humidity) ;
        this.data.feel = Math.round(result.main.feels_like) ;  
      }
    }
  }
};
</script>

<style scoped>
div,
button,
span,
strong,
p {
  font-family: "Open Sans";
}
#main{
  height:900px;
  background: url("./assets/sunny.jpg") no-repeat center center fixed; 
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;
   transition: 0.4s;
}

#main.cloudyImage{
   background-image: url("./assets/cloudy.jpg");
}
input[type=text] {
  height: 50px;
  border-top-left-radius: 20px;
  border-top-right-radius: 20px;
  background-color:rgba(0, 0, 0, .3);
  color:aqua;
}
input[type=text]:focus{
    background-color:rgba(0, 0, 0, .6);
    color: white;
}
.error-text {
  color: brown;
}
.line-height-min {
  line-height: 0.8;
}
.card-main{
  transition: 0.4s;
  background-color:rgba(0, 0, 0, .5) ;
  border-bottom-left-radius: 2rem;
  border-bottom-right-radius: 2rem;
} 
.temp-text {
  font-size: 60px;
  font-family: Georgia;
}
.bottom-text{
  font-size: 50px;
}
</style>
