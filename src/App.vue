<template>
  <div>
  <app-header :avatar="user.avatar" :name="user.userName"/>
      <main class="main">
        <section>
          <div class="container">
            <div class="row">
              <div class="col-12 col-md-4">
                <div class="profile">
                  <h1>Профиль</h1>
                  <div class="profile__img">
                    <img :src="user.avatar" alt="">
                  </div>
                  <div class="profile__name">
                    {{user.userName}}
                  </div>
                  <div class="profile__info">
                    {{getAge}}
                  </div>
                  <div class="profile__info">
                    {{user.job}}
                  </div>
                </div>
              </div>
              <div class="col-12 col-md-8">
                  <div class="info">
                    <div class="info__title">
                      Рекомендованное пиво
                    </div>
                    <div class="info__about about">
                      <div class="about__item">
                          <div class="about__title">
                            Название
                          </div>
                          <div class="about__value">
                              {{beer.name}}
                          </div>
                      </div>
                      <div class="about__item">
                          <div class="about__title">
                            Бренд
                          </div>
                          <div class="about__value">
                              {{beer.brand}}
                          </div>
                      </div>
                    </div>
                  <div class="info__charact charact">
                      <div class="charact__title">
                        Характеристики пива: 
                      </div>
                      <div class="charact__list">
                          <div class="charact__item" v-for="item, title in beer.characterics">
                            <span class="charact__name">
                              {{title}}:
                            </span>
                            <span class="charact__value">
                                {{item}}
                            </span>
                          </div>
                      </div>
                    </div>
                    <div class="info__change">
                      <button type="button" class="btn" @click="loadBeer()">
                        Поменять
                      </button>
                    </div>
                  </div>
              </div>
            </div>
          </div>
        </section>
      </main>
  </div>
</template>

<script>
import AppHeader from './components/Header.vue'
export default {
  components: {
    AppHeader
  },
  data: () => ({
    user: {
      avatar: "",
      userName: "",
      birth: "",
      job: ""
    },
    beer: {
      name: '',
      brand: '',
      characterics: {}
    }
  }),
  computed: {
    getArrBirth(){
      return this.user.birth.split('-').map(item => parseInt(item));
    },
    getYearBirth(){
      return this.getArrBirth[0];
    },
    getMonthBirth(){
      return this.getArrBirth[1];
    },
    getDayBirth(){
      return this.getArrBirth[2];
    },
    wasBirthDay(){
      let month = this.getMonthBirth;
      let day = this.getDayBirth;
      let currentDate = this.currentDate();

      if(month < currentDate[1])
      {
        return true;
      } else if(month == currentDate[1]){
        return day <= currentDate[2] ? true : false;
      }
      else{
        return false;
      }
    },
    getAge(){
      let age = this.currentDate()[0] - this.getYearBirth;
      return this.wasBirthDay ? age : age-1;
    }
    
  },
  methods: {
    currentDate(){
      let now = new Date();
      let year = now.getFullYear();
      let month = now.getMonth() + 1;
      let day = now.getDate();
      return [year, month, day ];
    },
    async loadUser(){
      fetch('https://random-data-api.com/api/users/random_user')
				.then((response) => {
					return response.json();
				})
				.then((data) => {
				  this.user.avatar = data.avatar;
          this.user.userName = data.first_name;
          this.user.birth = data.date_of_birth;
          this.user.job = data.employment.title;
				});
    },
    async loadBeer(){
      fetch('https://random-data-api.com/api/beer/random_beer')
				.then((response) => {
					return response.json();
				})
				.then((data) => {
				  this.beer.name = data.name;
          this.beer.brand = data.brand;
          let keys = ["id", "uid","name","brand"];
          let newObj = {};
          for (let key in data) {
            if (!keys.includes(key)) {
              newObj[key] = data[key];
            }
          }
          this.beer.characterics = newObj;
				});
    }
  },
  created(){
    this.loadUser();
    this.loadBeer();
  }
}
</script>

<style lang="scss">
  @import './assets/scss/_reset.scss';
  @import './assets/scss/_variables.scss';
  @import './assets/scss/_main.scss';
  .header{
    background-color: $mainColor;
    padding: 15px 0px;
    &__info{
      display: flex;
      align-items: center;
      margin-left: auto;
      justify-content: flex-end;
    }
    &__name{
        font-size: 18px;
        line-height: 22px;
        margin-right: 10px;
    }
    &__img{
        width: 50px;
        height: 50px;
        border-radius: 50%;
        border: 2px solid #000;
        img{
            width: 100%;
            height: 100%;
            border-radius: inherit;
            object-fit: cover;
            object-position: center;
        }
    }
  }   
  .main{
    background-image: url('./assets/img/bg.png');
    background-size: cover;
    background-position: center;
    height: calc(100vh - 80px);
    padding: 35px 0px;
    @include flexColumn();
    justify-content: center;
  }
  .profile{
    height: 100%;
    border-right: 2px solid #000;
    text-align: center;
    padding: 0px 38px;
    @include flexColumn();
    &__img{
      width: 150px;
      height: 150px;
      img{
        border-radius: 50%;
        width: 100%;
        height: 100%;
        border: 4px solid $mainColor;
        object-fit: cover;
        object-position: center;
      }
    }
    &__name{
      font-weight: 700;
      font-size: 28px;
      line-height: 34px;
    }
    &__info{
      font-size: 24px;
      line-height: 29px;
      color: $greyColor;
    }
  }
  .info{
    &__title{
      font-weight: 700;
      font-size: 24px;
      padding-bottom: 20px;
      border-bottom: 2px solid #000;
      margin-bottom: 40px;
    }
    &__about{
      @include flexRow();
      margin-bottom: 46px;
    }
    .about{
      &__item{
        width: 100%;
        max-width: 350px;
      }
      &__title{
        font-weight: 600;
        font-size: 16px;
        margin-bottom: 12px;
      }
      &__value{
        border: 2px solid $mainColor;
        border-radius: 7px;
        padding: 7px;
      }
    }
    &__charact{
      margin-bottom: 40px;
    }
    .charact{
      &__title{
        font-weight: 600;
        font-size: 20px;
        margin-bottom: 10px;
      }
      &__list{
        @include flexRow();
      }
      &__item{
        max-width: 30%;
        margin-bottom: 10px;
        font-size: 14px;
        line-height: 19px;
        width: 100%;
      }
      &__value{
        font-weight: 300;
      }
    }
  }
  @import './assets/scss/_media.scss';
</style>
