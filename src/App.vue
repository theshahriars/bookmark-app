<template>
  <div class="row mt-5 mb-4" id="app">
    <div class="col-md-3">
      <div class="list-group">
        <a href="javascript:void(0)" class="list-group-item text-white bg-success mb-2 text-center" data-toggle="modal" data-target="#create_modal">Add New Bookmark</a>
        <!--bookmark form modal-->
        <div class="modal fade" id="create_modal">
          <div class="modal-dialog">
            <form v-on:submit.prevent="addBookmark">
              <div class="modal-content">
                <div class="modal-header">
                  <h4 class="modal-title">Add New Bookmark</h4>
                  <button type="button" class="close" data-dismiss="modal">&times;</button>
                </div>
                <div class="modal-body">
                  <div class="form-group">
                    <label for="title">Title:</label>
                    <input
                      type="text"
                      class="form-control"
                      id="title"
                      name="title"
                      v-model="title"
                      required
                    >
                  </div>
                  <div class="form-group">
                    <label for="title">Link:</label>
                    <input
                      type="text"
                      class="form-control"
                      id="link"
                      name="link"
                      v-model="link"
                      required
                    >
                  </div>
                  <div class="form-group">
                    <label for="title">Note:</label>
                    <textarea
                      class="form-control"
                      id="note"
                      name="note"
                      cols="30"
                      rows="3"
                      v-model="note"
                      required
                    ></textarea>
                  </div>
                  <div class="form-group form-check">
                    <label class="form-check-label">
                      <input
                        class="form-check-input"
                        type="checkbox"
                        id="visited"
                        name="visited"
                        v-model="visited"
                      >
                      Visited
                    </label>
                  </div>
                </div>
                <div class="modal-footer">
                  <button type="submit" class="btn btn-success">Save</button>
                  <button type="button" class="btn btn-danger" data-dismiss="modal">Close</button>
                </div>
              </div>
            </form>
          </div>
        </div>
      </div>
      <div class="list-group">
        <!--sidebar option with filtering features-->
        <a href="javascript:void(0)" class="list-group-item text-dark" v-on:click="reloadData()">All</a>
        <a href="javascript:void(0)" class="list-group-item text-dark" v-on:click="filterVisited()">Visited</a>
        <a href="javascript:void(0)" class="list-group-item text-dark" v-on:click="filterNotVisited()">Not Visited</a>
      </div>
    </div>
    <div class="col-md-9">
      <!--bookmark component-->
      <Bookmark :bookmarks.sync="bookmarks"></Bookmark>
    </div>
  </div>
</template>

<script>
// importing Bookmark component
import Bookmark from './components/Bookmark'
// importing sweetalert module
import sweetalert from 'sweetalert'

export default {
  name: 'App',
  components: {
    // registering component
    Bookmark
  },
  data () {
    return {
      // defining variables
      bookmarks: [],
      title: '',
      link: '',
      note: '',
      visited: false
    }
  },
  methods: {
    addBookmark: function () {

      // pushing data in object
      this.bookmarks.push({
        title: this.title,
        link: this.link,
        note: this.note,
        visited: this.visited,
      })

      // storing data in localStorage
      window.localStorage.setItem('bookmark_app', JSON.stringify(this.bookmarks))

      // showing success alert
      sweetalert('Success!', 'Your bookmark has been added.', 'success');

      // clearing form fields
      this.title = ''
      this.link = ''
      this.note = ''
      this.visited = ''

      // reloading bookmarks
      this.reloadData()
    },
    filterVisited: function () {
      // reloading bookmarks
      this.reloadData()

      // collecting bookmarks data
      let data = this.bookmarks
      this.bookmarks = []

      // filtering bookmarks data
      data.forEach(singledata => {
        if (singledata.visited == true) {
          this.bookmarks.push(singledata)
        }
      })
    },
    filterNotVisited: function () {

      // reloading bookmarks
      this.reloadData()

      // collecting bookmarks
      let data = this.bookmarks
      this.bookmarks = []

      // filtering bookmarks
      data.forEach(singledata => {
        if (singledata.visited == false) {
          this.bookmarks.push(singledata)
        }
      })
    },
    reloadData: function () {
      let all_bookmarks = JSON.parse(window.localStorage.getItem('bookmark_app') || '{}')
      if (all_bookmarks.length > 0)
      {
        this.bookmarks = all_bookmarks
      }
    }
  },
  created () {
    this.reloadData()
  }
}
</script>

<style>
</style>
