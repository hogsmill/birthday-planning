<template>
  <div id="app" class="mb-4">
    <h1>Guests</h1>
    <div class="controls">
      <button @click="load()">Load</button>
      <button @click="save()">Save</button>
    </div>
    <div class="people">
      <span>{{countGuests()}} guests</span>
      <div class="guests" v-for="(guest, index) in guests" :key="index">
        <button @click="moveGuestUp(index)" :disabled="index == 0">&uarr;</button>
        <button @click="moveGuestDown(index)" :disabled="index == guests.length - 1">&darr;</button>
        <div class="guest">{{guest}}</div>
        <button @click="show('before', index)">Add Before</button>
        <button @click="show('after', index)">Add After</button>
        <button @click="deleteGuest(index)">Delete</button>
        <hr v-if="index == 14" />
      </div>
    </div>

    <modal name="add-guest" :height="140">
      <div class="text-right"><span @click="hide" class="glyphicon glyphicon-star">x</span></div>
      <h4>Add Guest {{positionString}}</h4>
      <div class="add-guest">
        <input type="text" id="guest" class="form-control" />
        <button class="btn btn-sm btn-info" @click="addGuest">Add</button>
      </div>
    </modal>

  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      position: 0,
      positionWhere: '',
      positionString: '',
      guests: []
    }
  },
  methods: {
    show (position, index) {
      this.positionString = position + " '" + this.guests[index] + "'"
      this.position = index
      this.positionWhere = position
      this.$modal.show('add-guest');
    },
    hide () {
      this.$modal.hide('add-guest');
    },
    load() {
      console.log("loading")
      this.guests = JSON.parse(localStorage.getItem("guests"))
      console.log(this.guests)
    },
    save() {
      console.log("saving")
      localStorage.setItem("guests", JSON.stringify(this.guests))
    },
    countGuests() {
      var total = 0
      for (var i = 0; i < this.guests.length; i++) {
        if (this.guests[i].match(/ and /)) {
          total += 2
        } else {
          total += 1
        }
      }
      return total
    },
    addGuest() {
      var guest = document.getElementById("guest").value
      if (this.positionWhere == 'before') {
        this.guests.splice(this.position, 0, guest)
      } else {
        this.guests.splice(this.position + 1, 0, guest)
      }
      this.hide()
    },
    moveGuestUp(index) {
      var guest = this.guests[index - 1]
      this.guests.splice(index - 1, 1)
      this.guests.splice(index, 0, guest)
    },
    moveGuestDown(index) {
      var guest = this.guests[index]
      this.guests.splice(index, 1)
      this.guests.splice(index + 1, 0, guest)
    },
    deleteGuest(index) {
      this.guests.splice(index, 1)
    }
  }
}
</script>

<style>
  button { border: none; }
  .controls { text-align: right; }
  .guest { border: 1px solid #ccc; margin: 2px 4px; width: 200px; display: inline-block; }
  input#guest { width: 200px; margin: 0 auto; }
</style>
