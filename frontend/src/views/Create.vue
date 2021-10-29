<template>
    <div class='create-page body-content'>
        <div class='left-block'>
          <div class="cardblock">
            <div class="namecard">
              <p class="name2">{{this.$store.state.card_name}}</p>
              <span class="phonetic2">{{this.$store.state.card_furigana}}</span>

              <div class="contact">
                <span><i class="fas fa-school">{{this.$store.state.card_affiliation}}</i></span>
                <span><i class="fas fa-envelope">{{this.$store.state.email}}</i></span>
                <span><i class="fas fa-school">{{this.$store.state.birthday}}</i></span>
                <span><i class="fas fa-heart">{{this.$store.state.card_favourite}}</i></span>
                <span><i class="fab fa-github-square">{{this.$store.state.card_skills}}</i></span>
              </div>
              <div class="namecard-img1">
                <img :src=this.url alt="顔画像" style="bottom: 13.0rem">
              </div>
          </div>
        </div>
        </div>
        <div class='right-block'>
            <form  name='create_form' id='contact-form' class='form-horizontal' role='form'>
            <div class='form-group '>
                <div class='col-sm-12'>
                <input type='text' class='form-control form-category' id='name' placeholder='NAME' name='name' v-model='FORM_NAME'>
                </div>
            </div>

            <div class='form-group'>
                <div class='col-sm-12'>
                    <input type='text' class='form-control form-category' id='furigana' placeholder='FURIGANA' name='furigana' v-model='FORM_FURIGANA' required>
                </div>
            </div>
            <div class='form-group'>
                <div class='col-sm-12'>
                    <input type='date' class='form-control form-category' id='birthday' placeholder='BIRTHDAY' name='birthday' v-model='FORM_BIRTHDAY' required>
                </div>
            </div>
            <div class='form-group'>
                <div class='col-sm-12'>
                    <input type='text' class='form-control form-category' id='favourite' placeholder='FAVOURITE' name='favourite' v-model='FORM_FAVOURITE' required>
                </div>
            </div>
            <div class='form-group'>
                <div class='col-sm-12'>
                    <input type='text' class='form-control form-category' id='skills' placeholder='SKILLS' name='skills' v-model='FORM_SKILLS' required>
                </div>
            </div>
            <div class='form-group'>
                <div class='col-sm-12'>
                    <input type='text' class='form-control form-category' id='affiliation' placeholder='AFFILIATION' name='affiliation' v-model='FORM_AFFILIATION' required>
                </div>
            </div>
            <div class='form-group'>
                <div class='col-sm-12'>
                    <input @change="selectedFile" type="file" name="file">
                </div>
            </div>
               <!-- <textarea class='form-control user-detail' v-model='FORM_FAVOURITE' rows='10' placeholder='FAVOURITE' name='favourite' required style='font-family: 'Yu Gothic medium', '游ゴシック Medium', Yugothic, '游ゴシック体', 'ヒラギノ角 Pro W3', sans-serif;'></textarea>-->
                <a class='button registor' @click='Create' >登録</a>
            </form>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
// import Card1 from './card1'
import firebase from 'firebase/compat/app'
import 'firebase/compat/storage'

export default {
  // components: { Card1 },
  name: 'Create',
  data () {
    return {
      uploadFile: '',
      FORM_NAME: '',
      FORM_FURIGANA: '',
      FORM_BIRTHDAY: '',
      FORM_FAVOURITE: '',
      FORM_SKILLS: '',
      FORM_AFFILIATION: '',
      imgname: '',
      url: '',
      myresponse: ''
    }
  },
  methods: {
    Create: async function () {
      var detail = [{
        imgpath: this.imgname,
        name: this.FORM_NAME,
        furigana: this.FORM_FURIGANA,
        birthday: this.FORM_BIRTHDAY,
        favourite: this.FORM_FAVOURITE,
        skills: this.FORM_SKILLS,
        email: this.$store.state.card_email,
        affiliation: this.FORM_AFFILIATION,
        uID: this.$store.state.uID
      }]
      console.log(detail)
      // Firebase上のストレージに保存
      const storageRef = await firebase.storage().ref('images/' + this.imgname)
      await storageRef.put(this.uploadFile).then(() => {
        console.log('uploaded ' + this.imgname)
      })

      await axios.post('http://127.0.0.1:5000/api/design', detail).then(response => {
        this.$store.dispatch('createCard', {
          card_name: response.data.name,
          card_imgpath: response.data.imgpath,
          card_furigana: response.data.furigana,
          card_birthday: response.data.birthday,
          card_favourite: response.data.favourite,
          card_skills: response.data.skills,
          card_email: response.data.email,
          card_affiliation: response.data.affiliation,
          card_code: response.data.card_code,
          uID: this.$store.state.uID
        })
        console.log(response.data)
        this.CARD_NAME = response.data.name
        this.CARD_FURIGANA = response.data.furigana
        this.CARD_BIRTHDAY = response.data.birthday
        this.CARD_FAVOURITE = response.data.favourite
        this.CARD_SKILLS = response.data.skills
        this.FORM_NAME = ''
        this.FORM_FURIGANA = ''
        this.FORM_BIRTHDAY = ''
        this.FORM_FAVOURITE = ''
        this.FORM_SKILLS = ''
        this.FORM_EMAIL = ''
        this.FORM_AFFILIATION = ''
        console.log(this.url)
      }).catch(err => {
        console.log(err)
      })

      const storageRef2 = await firebase.storage().ref('images/' + this.$store.state.card_imgpath)
      await storageRef2.getDownloadURL().then(url => {
        console.log(url)
        this.url = url
        console.log(this.url)
      })
    },
    selectedFile: function (e) {
      e.preventDefault()
      this.uploadFile = e.target.files[0]
      console.log(e.target.files[0].type)
      console.log(String(e.target.files[0].type).slice(6))
      this.imgname = this.$store.state.uID + '.' + String(e.target.files[0].type).slice(6)
      console.log(this.imgname)
    }
  },
  created () {
    const storageRef3 = firebase.storage().ref('images/' + this.$store.state.card_imgpath)
    storageRef3.getDownloadURL().then(url => {
      console.log(url)
      this.url = url
      console.log(this.url)
    })
  }
}
</script>

<style>
.registor{
    left:210px
}
.create-page{
    margin-top:66px;
    display: flex;
}
.right-block{
  text-align:center;
  width:40%;
}
.left-block{
  width:60%;
}
.form-horizontal {
  /*float: left;*/
  width: 400px;
  position:relative;
}
.user-detail{
    height:200px;
}
.form-category{
  height:40px;
  margin-bottom:10px;
}
.form-control,
textarea {
  padding-left:7px;
  font-size:20px;
  resize:none;
  width: 500px;
  background-color: #fff;
  color: #312F2F;
  letter-spacing: 1px;
  border-radius:10px;
  border-width:5px;
  outline :none
}
.user-detail{
  padding-top:10px;
}
.send-button {
  margin-top: 15px;
  height: 34px;
  width: 400px;
  overflow: hidden;
  transition: all .2s ease-in-out;
}

.alt-send-button {
  width: 400px;
  height: 34px;
  transition: all .2s ease-in-out;
}

.send-text {
  display: block;
  margin-top: 10px;
  font: 700 12px 'Lato', sans-serif;
  letter-spacing: 2px;
}

.alt-send-button:hover {
  transform: translate3d(0px, -29px, 0px);
}
</style>
