
<template>
<div>
  <div v-if="currentBaby">
    <h5 class="main-title text-center color-pink mt-3">{{currentBaby.baby_name}}의 포토카드</h5>
    <!-- Date Picker -->
    <!-- <v-row class="d-flex justify-content-center no-gutters">
      <v-col cols="3" sm="3" class="erase-padding">
        <v-dialog
          ref="dialog"
          v-model="modal"
          :return-value.sync="date"
          width="290px"
        >
          <template v-slot:activator="{ on, attrs }">
            <v-text-field
              v-model="date"
              readonly
              v-bind="attrs"
              v-on="on"
              class="centered-date"
            ></v-text-field>
          </template>
          <v-date-picker color="#FEA59C" v-model="date" type="month" scrollable>
            <v-spacer></v-spacer>
            <v-btn text color="#9BC7FF" @click="modal = false">Cancel</v-btn>
            <v-btn text color="#9BC7FF" @click="$refs.dialog.save(date)">OK</v-btn>
          </v-date-picker>
        </v-dialog>
      </v-col>
    </v-row> -->

    <!-- Swiper -->
    <div v-if="photoDiaries" class="mt-5">
      <swiper class="swiper" :options="swiperOption" v-if="photoDiaries.length">
        <swiper-slide v-for="diary in photoDiaries" :key="diary.id">
          <div class="card" v-if="diary.cover_photo" @click="clickCard(diary.id)" :style="`background-image:url(${diary.cover_photo})`">
            <div class="p-3 title col-3 text-center">
              <span class="date">{{diary.diary_date | moment("DD")}}</span><br>
              <span class="month">{{ diary.diary_date | moment("MMM") }}</span>
            </div>
            <div class="p-3 mt-auto">
              <h4 class="diary-text">{{diary.title}}</h4>
            </div>
            <!-- <img src="https://lh3.googleusercontent.com/proxy/I8VHuGeCKm3Mr0766qrIhVU1CRQCtWKWNhdI-qjWprYYF5Ov20qyr8NmuJR-k6avovYBkit3UERqwGpOLyDJtrYX5GKN3EcFZMkVurpYgz81rzR_tXULTeU72fAoP6Z_tW24KCOrxgWIbb14HITOhg" class="card-img-top " alt="..."> -->
          </div>
        </swiper-slide>
        <div class="swiper-pagination" slot="pagination"></div>
      </swiper>
      <div v-else class="text-center no-photos mt-5">
         <!-- 만약 업로드 된이미지가 없을 경우 -->
        <img class="crying-baby" src="@/assets/baby.png">
        <h5>아직 작성된 포토 다이어리가 없습니다.<br>아기 사진과 함께 다이어리를 작성해주세요!</h5>
     </div>
    </div>
  </div>
</div>
</template>

<script>

import { Swiper, SwiperSlide } from 'vue-awesome-swiper'
import 'swiper/swiper-bundle.css'

import { mapState, mapActions } from 'vuex'
 

export default {
  name: 'DiaryPhoto',
  components: {
    Swiper,
    SwiperSlide,
  },
  computed: {
    ...mapState(['myaccount', 'currentBaby']),
    ...mapState('diaryStore', ['photoDiaries'])
  },
  data() {
    return {
      swiperOption: {
        slidesPerView: 1,
        spaceBetween: 30,
        centeredSlides: true,
        pagination: {
          el: '.swiper-pagination',
          clickable: true
        }
      },
      selectedDate: '',
      date: new Date().toISOString().substr(0, 7),
      menu: false,
      modal: false,
    }
  },
  methods: {
    ...mapActions('diaryStore', ['fetchPhotoDiaries']),
    clickAdd() {
      this.$router.push({name: 'DiaryCreate'})
    },
    clickCard(diaryId){
      this.$router.push({name: 'DiaryDetail', params: { 'diaryId': diaryId } })
    }
  },
  created() {
    this.fetchPhotoDiaries()
  }
}
</script>

<style scoped>
.crying-baby {
  height: 30vh;
  width: auto;
}

p {
  margin: 0;
}

.main-title {
  font-size: 1.3rem;
  font-weight: 900;
}

.card {
  height: 60vh;
  width: 82vw;
  background-size: cover;
  background-position: 50% 50%;
  border-radius: 10%;
  box-shadow: 10px 10px  5px rgba(0,0,0,0.6);
  -moz-box-shadow: 10px 10px  5px rgba(0,0,0,0.6);
  -webkit-box-shadow: 10px 10px  5px rgba(0,0,0,0.6);
  -o-box-shadow: 10px 10px  5px rgba(0,0,0,0.6);
  margin-bottom: 2vh;
  margin-left: auto;
  margin-right: auto;
}


.title {
  font-weight: 900;
  font-size: 1.6rem;
  color: white;
}

.title .date {
  font-size: 2.5rem;
  text-shadow: 5px 5px 20px rgba(0,0,0,0.6);
}

.month {
  text-shadow: 5px 5px 20px rgba(0,0,0,0.6);
}

/* date picker */
.erase-padding {
  padding: 0;
}

.v-text-field > .v-input__control > .v-input__slot {
  margin: 0 !important;
}

.v-input {
  padding: 0 !important;
}

.centered-date >>> input {
  text-align: center !important;
}

#input-14 {
  padding: 0 !important;
  text-align: center !important;
}

.diary-text {
  color: white;
  text-shadow: 2px 2px black;
}

/* 추가 버튼 */
.add-button {
  position: fixed;
  bottom: 10vh;
  right: 5vw;
  text-shadow: 5px 5px 20px rgba(0,0,0,0.6);
  z-index: 1000;
}

</style>