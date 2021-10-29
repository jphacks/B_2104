<template>
    <div>
    <div class='create-page body-content'>
        <div class='left-block'>
          <div class="cardblock">
            <div class="namecard">
              <p class="name2">{{this.name}}</p>
              <span class="phonetic2">{{this.furigana}}</span>
              <div class="contact">
                <span><i class="fas fa-school">{{this.$store.affiliation}}</i></span>
                <span><i class="fas fa-envelope">{{this.email}}</i></span>
                <span><i class="fas fa-school">{{this.birthday}}</i></span>
                <span><i class="fas fa-heart">{{this.favourite}}</i></span>
                <span><i class="fab fa-github-square">{{this.skills}}</i></span>
              </div>
              <div class="namecard-img1">
                <img :src=this.imgpath alt="顔画像" style="bottom: 13.0rem">
              </div>
          </div>
        </div>
        </div>
    </div>
    <div>
     <p></p>
    </div>
    <div>
      <a class="button" @click='back'>back</a>
    </div>
    </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Showcard',
  head: {
    link: [
      { rel: 'stylesheet', href: 'https://use.fontawesome.com/releases/v5.6.3/css/all.css' }
    ]
  },
  data () {
    return {
      name: '',
      furigana: '',
      birthday: '',
      favourite: '',
      skills: '',
      email: '',
      affiliation: '',
      imgpath: ''
    }
  },
  mounted () {
    console.log('start')
  },
  methods: {
    back: function () {
      this.$router.push('/gallery')
    }
  },
  created () {
    var detail = [{
      uID: this.$store.state.show_card
    }]
    console.log(this.ID)
    axios.post('http://127.0.0.1:5000/api/show_card', detail).then(response => {
      console.log(response.data)
      this.name = response.data.name
      this.furigana = response.data.furigana
      this.birthday = response.data.birthday
      this.favourite = response.data.favourite
      this.skills = response.data.skills
      this.email = response.data.email
      this.affiliation = response.data.affiliation
      this.imgpath = response.data.imgpath
    }).catch(err => {
      alert('カードの作成を行ってください')
      console.log(err)
    })
  }
}
</script>
<style>
.namecard {
            text-align:left;
            background-image:url("../images/background12.png");
            background-size: cover;
            background-repeat: no-repeat
}
</style>
