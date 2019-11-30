<template>
  <div id="app">
    <FadingImage
      v-for="(img, index) in images"
      :imgSrc="img.representation[0].id"
      :key="img.id"
      v-on:breed="onBreed"
      v-on:remove="onRemove(index)"
    />
  </div>
</template>

<script>
import FadingImage from "./components/FadingImage";
import JQuery from "jquery";
let $ = JQuery;

export default {
  name: "App",
  components: {
    FadingImage
  },
  data: function() {
    return {
      images: [
        {
          "@context": "https://linked.art/ns/v1/linked-art.json",
          id: "http://hdl.handle.net/10934/RM0001.COLLECT.326",
          representation: [
            {
              _label: "Rijksmuseum Image API",
              id:
                "https://lh3.googleusercontent.com/_DBa0zX8Vx1t8IA2oOyOnWito00trZIY7XabnX8QyKG3VSsTvNuc8hq9jyoNgPz--O9z-L71QQ-vWnQOwoDtUYjBRWkc=s0",
              type: "VisualItem"
            }
          ],
          type: "HumanMadeObject"
        },
        {
          "@context": "https://linked.art/ns/v1/linked-art.json",
          id: "http://hdl.handle.net/10934/RM0001.COLLECT.327",
          representation: [
            {
              _label: "Rijksmuseum Image API",
              id:
                "https://lh3.googleusercontent.com/3lZ51I00AIa5KZ7g0ta82UAOJsHV_Tab_OnVqTcLx_Wi2Un22F-tZzWkwcYWOqoaLEInUfce9GVIWnKfbcBkdAgcKZE=s0",
              type: "VisualItem"
            }
          ],
          type: "HumanMadeObject"
        }
      ]
    };
  },
  methods: {
    onBreed() {
      console.log("birth!");
    },
    onDeath() {
      console.log("death!");
    },
    onRemove(index) {
      console.log("REMOVE");
      this.images.splice(index, 1);
    },
    randomIntFromInterval(min, max) {
      // min and max included
      return Math.floor(Math.random() * (max - min + 1) + min);
    },
   get_data(url, data_to_send) {
    var tmp = null;
    $.ajax({method: 'GET',
            async: false,
            url: url, 
            data: data_to_send,
            xhrFields: {
              withCredentials: false
            },
            success: function(data, status) {
             tmp = data;
          }})
    if(tmp !== undefined && tmp.length !== null) {
      return tmp
    }
  },

  created: function() {
    const URL = "http://172.16.45.236:5000/seed";
    // const response_uri = "http://hdl.handle.net/10934/RM0001.COLLECT.321537";
    // const response_url =
    //   "https://lh3.googleusercontent.com/S3wgRasrmmAkDv4PrS1TkUSvnVL4ve-Gpv6oIhutvCCqlqvayWaqBYbnSrlE6X5HrJl2x5yLQuIgm-MO7n3uWdPUOg=s0";
    // // console.l'g("crea'ed");
    // $.get(URL, { uri: response_uri, url: response_url }, function(
    //     data,status) {
    //     this.images = data;
    //     console.log(data);
    //     console.log(status);
    //   }
    // );
    // console.log(this.images);
    this.messsages = get_data(URL, {hello: "world!"})
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
