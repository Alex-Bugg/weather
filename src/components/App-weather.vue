<template>
  <div class="weather warm">
    <input @keydown="search" v-model="city" class="weather_input" type="text" placeholder="Search...">
    <div class="weather_day_info" v-if="typeof weather.main != 'undefined'">
      <div class="day_info_all">
        <h2 class="weather_name">{{weather.name}}</h2>
        <p class="weather_time">{{dateBuilder()}}</p>
        <img class="day_info_all_img" :src='ico'>
        <p class="weather_time">{{weather.weather[0].main}}</p>
        <div class="day_info_all_temp">{{Math.floor(weather.main.temp)}}°C</div>
        <table class="table">
          <tr class="table_line">
            <td class="table_point">Feels like</td>
            <td class="table_point">{{Math.floor(weather.main.feels_like)}}</td>
          </tr>
          <tr class="table_line">
            <td class="table_point">Wind speed</td>
            <td class="table_point">{{weather.wind.speed}}</td>
          </tr>
          <tr class="table_line">
            <td class="table_point">Humidity</td>
            <td class="table_point">{{weather.main.humidity}}</td>
          </tr>
          <tr class="table_line">
            <td class="table_point">Pressure</td>
            <td class="table_point">{{weather.main.pressure}}</td>
          </tr>
        </table>
      </div>
    </div>
    <div v-if="error" class="error">Somethink wrong... Try again!</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      city: '',
      url: 'https://api.openweathermap.org/data/2.5/',
      key: 'e9f517c620c09ab33641cac8d8d53e73',
      weather: {},
      ico: '',
      urlIco:'http://openweathermap.org/img/wn/',
      error: false
    }
  },
  methods: {
    search(e) {
      if(e.key === 'Enter') {
        fetch(`${this.url}weather?q=${this.city}&units=metric&appid=${this.key}`)
        .then(res => {
          return res.json()
        }).then( data => {
          this.setResults(data)
          this.fetchIco(data)
          this.error = false
          console.log(this.weather)
        }).catch(error => {
          this.error = true
        })
      }
    },
    setResults(res) {
      this.weather = res
    },
    fetchIco(data) {
      fetch(`${this.urlIco}${data.weather[0].icon}.png`)
      .then(res => {
        this.ico = res.url
      })
    },
    dateBuilder () {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    } 
  },
}
</script>

<style lang="scss">
  .warm {
    background-image: url('../assets/warm-bg.jpg');
  }
  .cold {
    background-image: url('../assets/cold-bg.jpg');
  }
  .weather {
    width: 460px;
    min-height: 600px;
    border-radius: 10px;
    box-shadow: 1px 1px 19px 0px rgba(0, 0, 0, 0.88);
    background-color: #fff;
    font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 10px;
    background-repeat: no-repeat;
    background-size: cover;
    &_input {
      background-color: rgba(255,255,255, 0.3);
      width: 300px;
      padding: 15px;
      border-radius: 16px 0 16px 0;
      font-size: 18px;
      font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
      transition: 0.2s;
      color: ivory;
      opacity: 0.7;
      margin-bottom: 15px;
      &::placeholder {
        color: ivory;
      }
      &:focus {
      border-radius:0 16px 0 16px ;
      background-color: rgba(255,255,255, 0.4);
      opacity: 1;
      }
    }
    &_name {
      font-size: 45px;
      color: ivory;
      font-weight: bold;
      text-shadow: 3px 3px rgba(0,0,0, 0.5);
    }
    &_time {
      color: ivory;
      font-style: italic;
      text-shadow: 3px 3px rgba(0,0,0, 0.5);
      padding: 10px;  
      margin-bottom: 10px;
    }
    &_cards_wrapper {
      display: flex;
      justify-content: space-between;
    }
  }
  .day_info_all {
    display: flex;
    align-items: center;
    flex-direction: column;
    &_temp {
      font-size: 70px;
      color: ivory;
      background-color: rgba(255,255,255,0.4);
      padding: 10px;
      border-radius: 5px;
      text-shadow: 3px 3px rgba(0,0,0, 0.5);
      font-weight: bold;
      margin-bottom: 50px;
      box-shadow: 3px 3px 10px rgba(0, 0, 0, 0.4);
    }
  }
  .table {
    background-color: rgba(255,255,255,0.4);
    padding: 10px;
    border-radius: 16px 0 16px 0;
    width: 300px;
    transition: 0.2s;
    font-weight: bold;
    &:hover {
      border-radius: 0 16px 0 16px;
    }
    &_line {
    }
    &_point {
      padding: 5px;
      font-size: 20px;
      text-align: center;
      border-bottom: 1px solid rgba(0, 0, 0, 0.3);
      color: ivory;
      text-shadow: 2px 2px rgba(0, 0, 0, 0.7);
    }
  }
  .error {
    font-size: 20px;
    color: ivory;
    background-color: rgba(238, 15, 15, 0.4);
    padding: 10px;
    border-radius: 5px;
    text-shadow: 3px 3px rgba(0,0,0, 0.5);
    font-weight: bold;
    margin-bottom: 50px;
    box-shadow: 3px 3px 10px rgba(0, 0, 0, 0.4);
  }
</style>