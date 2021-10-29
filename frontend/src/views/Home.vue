<template>
  <div class="home-content wrapper body-content">
    <article>
        <header class="page-info">
            <h2 class="page-title">NameCa</h2>
        </header>
        <p>カジュアルな名刺交換を体験してみませんか</p>
        <a class="button" @click='Toaddcard'>Share</a>
        <a class="button2" @click='ToGallery'>Gallery</a>
    </article>
    <aside>
        <img src="../images/tree2.png" alt="ホームの木">
    </aside>
  </div><!-- /.home-content -->
</template>

<script>
import axios from 'axios'
export default {
  name: 'home',
  data () {
    return {
      card_code: '',
      uID: ''
    }
  },
  methods: {
    show: function () {
      this.$modal.show('card_code')
    },
    hide: function () {
      this.$modal.hide('card_code')
    },
    cardcode_update: function () {
      var detail = [{
        card_code: this.card_code,
        uID: this.uID
      }]
      axios.post('http://127.0.0.1:5000/api/card_code', detail).then(response => {
        console.log(response.data.card_code)
        if (response.data.flag === 1) {
          alert('このコードは既に使用されています')
        } else {
          this.$store.dispatch('create_code', { Cardcode: response.data.card_code })
          this.card_code = this.$store.state.card_code
          this.hide()
        }
      }).catch(err => {
        alert('カードの作成を行ってください')
        console.log(err)
      })
    },
    Toaddcard: function () {
      this.$router.push('/addcard')
    },
    ToGallery: function () {
      this.$router.push('/gallery')
    }
  },
  created () {
    this.card_code = this.$store.state.card_code
    this.uID = this.$store.state.uID
    console.log(this.uID)
    console.log(this.card_code)
  }

}
</script>
