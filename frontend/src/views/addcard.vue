<template>
<div>
  <div class="point-box">
    <div class="point-title">自分のID</div>
    <div class="boxContainer">
      <div class="box"><img class="logo" src="../images/ユーザ.png" alt="ユーザ画像"></div>
      <div class="box">{{this.$store.state.card_code}}</div>
    </div>
  </div>
  <div class="point-box">
    <div class="point-title">相手のIDを入力してください</div>
      <div class="boxContainer">
        <div class="box"><img class="logo" src="../images/フレンド.png" alt="フレンド追加画像"></div>
        <div class="box">
          <form  name="contact-form">
            <input type="text" v-model='card_code' placeholder="ID">
          </form>
        </div>
      </div>
    </div>
    <div class="boxContainer2">
      <a class="button3" @click='addcard'>Serch</a>
    </div>
</div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'addcard',
  data () {
    return {
      card_code: '',
      uID: ''
    }
  },
  mounted () {
  },
  methods: {
    addcard: function () {
      var detail = [{
        card_code: this.card_code,
        uID: this.uID
      }]
      console.log(this.uID)
      axios.post('http://127.0.0.1:5000/api/add_gallery', detail).then(response => {
        alert(response.data.addname)
      }).catch(err => {
        alert('名刺が見つかりませんでした')
        console.log(err)
      })
    }
  },
  created () {
    this.uID = this.$store.state.uID
  }
}
</script>
