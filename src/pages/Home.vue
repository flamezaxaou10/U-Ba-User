<template>
  <div class="Login container">
  <br>
    <div class="columns is-centered ">
      <div class="column is-6 is-centered has-text-centered">
              <h3 class="is-size-1">Uba-User</h3>
        <div class="google-map">
          <gmap-map :center="center" :zoom="10" style="width: 100%; height: 450px">
            <gmap-marker :key="index" v-for="(m, index) in markers" :position="m.position" :clickable="true" :draggable="true" @click="center=m.position">
            </gmap-marker>
          </gmap-map>
        </div>
        <hr>
        <div class="columns">
          <div class="column is-4">
            <img :src="user.fb.photoURL" alt="" width="100px" height="100px" style="border-radius:50%;"><br>
            <button type="button" name="button"class="button is-danger is-right" @click="logout()">Logout</button>
          </div>
          <div class="column is-8">
            ID : {{user.fb.uid}} <br>
            Name : {{user.displayName}} <br>
            {{latitude}} : {{longitude}}
            <br> <br>
            <button type="button" class="button is-link is-medium" style="width:80%;" @click="call()">Calling UBa</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
// var apiKey = 'AIzaSyDn7zvKP5RUc6FZnQFacNnH0TW1x1EE_qU'
import { mapActions, mapGetters } from 'vuex'
export default {
  name: 'Login',
  data () {
    return {
      latitude: 0,
      longitude: 0,
      msg: 'Header',
      center: {lat: 13.754, lng: 100.5014},
      markers: [{
        position: {lat: 13.754, lng: 100.5014}
      }]
    }
  },
  computed: {
    ...mapGetters([
      'user',
      'isReady'
    ])
  },
  methods: {
    ...mapActions([
      'login',
      'logout',
      'call'
    ]),
    getLocation () {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(this.showPosition)
      }
    },
    showPosition (position) {
      this.latitude = position.coords.latitude
      this.longitude = position.coords.longitude
      this.center.lat = position.coords.latitude
      this.center.lng = position.coords.longitude
      this.markers[0].position.lat = position.coords.latitude
      this.markers[0].position.lng = position.coords.longitude
    }
  },
  mounted () {
    this.getLocation()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .google-map {
    width: 100%;
    height: 450px;
    margin: auto;
    margin-bottom: 10px;
    background-color: grey;
  }
</style>
