<template>
    <div id='gallery' class='container'>
        <h1>Gallery</h1>
        <div class='wrapper grid'>
          <div v-for='(person, index) in people' v-bind:key='index' class='item'>
            <img style='width: 100px; height: 100px' :src='person.imgpath'/>
            <p><button v-on:click='showCard(index)'>{{ person.name }}</button></p>
          </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import firebase from 'firebase/compat/app'
import 'firebase/compat/storage'
export default {
  name: 'gallery',
  data () {
    return {
      people: [],
      uID: '',
      url: ''
    }
  },
  mounted () {
    this.readinfo()
  },
  methods: {
    showCard: function (index) {
      var cardID
      cardID = this.people[index].id
      this.$store.dispatch('show_card', { showCard: cardID })
      this.$router.push('/showcard')
    },
    readinfo: function () {
      this.uID = this.$store.state.uID
      console.log(this.uID)
      var detail = [{
        uID: this.$store.state.uID
      }]
      axios.post('http://127.0.0.1:5000/api/show_gallery', detail).then(response => {
        console.log(response.data)
        var data = {
          id: '',
          name: '',
          imgpath: ''
        }
        for (var i in response.data) {
          var storageRef = firebase.storage().ref('images/' + response.data[i].imgpath)
          storageRef.getDownloadURL().then(url => {
            this.url = url
            console.log(this.url)
          })
          data = {
            id: response.data[i].id,
            name: response.data[i].name,
            imgpath: this.url
          }
          console.log(data)
          this.people.push(data)
          console.log(this.people)
        }
      }).catch(err => {
        console.log(err)
      })
    }
  }
}
</script>
