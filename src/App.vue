<template>
  <div id="app">

    <div id="gui_container">
      <div id="gui">
          <h1> Niet te Vergeten
           </h1>

      </div>
    </div>

    <div id="images_container">

    <FadingImage
      v-for="(img, index) in images"
      :imgURL="img.representation[0].id"
      :imgURI="img.id"
      :key="img.id+Math.random().toString()"
      :index="index"
      :xpos="img.xpos"
      :ypos="img.ypos"
      v-on:breed="onBreed"
      v-on:remove="onRemove"
    />
  </div>
  </div>
</template>

<script>
import FadingImage from "./components/FadingImage";
import vispaSampleImages from "./assets/vispa-sample-response.json";
import JQuery from "jquery";
let $ = JQuery;

export default {
  name: "App",
  components: {
    FadingImage
  },
  data: function() {
    return {
      sampleImages: vispaSampleImages,
      images: [],
      LODURLs: [
        "https://v2.bencomp.nl/year",
        "https://v2.bencomp.nl/location",
        "https://v2.bencomp.nl/technique",
      ],
      padding: 200,
      noStartImages: 3,
    };
  },
  methods: {
    onBreed(xpos, ypos, parent_url, parent_uri) {
      // Breed child image that is visually similar

      // const URL = "http://172.16.45.236:5000/vispa";
      // let newImage = this.get_data(URL, {uri: parent_uri, url: parent_url})[0];
      let sampleImages = this.get_data_temp()
      let randomImage = sampleImages[Math.floor(Math.random()*sampleImages.length)]
      let newImage = randomImage
      // TODO set origin- class based on origin of returned URI

      // give newImage a position near its parent
      newImage.xpos = xpos + this.randomIntFromInterval(-this.padding, this.padding)
      newImage.ypos = ypos + this.randomIntFromInterval(-this.padding, this.padding)
      this.images.push(newImage)
    },
    onRemove(index, imgURL, imgURI) {
      this.$delete(this.images, index);
    },
    onSpawn(parent_url, parent_uri){ 
      // Spawn new image that is similar based on LOD metadata

      // let URL = this.LODURLs[this.randomIntFromInterval(0,2)];
      let URL = this.LODURLs[0]; // stick to year API for now
      let newImage = this.get_data(URL, {uri: parent_uri, url: parent_url})[this.randomIntFromInterval(0,5)];
      // console.log(newImage);
      this.images.push(newImage);
    },
    randomIntFromInterval(min, max) {
      // min and max included
      return Math.floor(Math.random() * (max - min + 1) + min);
    },
    get_data(url, data_to_send) {
      ///////////////////////////////////////////////////////
      // temporarily hard code response til API is working //
      //////////////////////////// //////////////////////////

      var tmp = null;
      $.ajax({method: 'GET',
              async: false,
              url: url, 
              data: data_to_send,
              xhrFields: {
                withCredentials: false
              },
              // eslint-disable-next-line
              success: function(data, status) {
               tmp = data;
            }})
      if(tmp !== undefined && tmp.length !== null) {
        return tmp
      }
    },
    get_random_pos() {
      return {
        'xpos': this.randomIntFromInterval(this.padding, window.innerWidth - this.padding),
        'ypos': this.randomIntFromInterval(this.padding, window.innerHeight - this.padding)
      }
    },
    get_data_temp() {
      ///////////////////////////////////////////////////////
      // temporarily hard code response til API is working //
      //////////////////////////// //////////////////////////
      const sampleImages = this.sampleImages
      // add random xpos and ypos to images
      let sampleImagesWPos = sampleImages.map(image => {
        return {...image, ...this.get_random_pos()}
      })
      return sampleImagesWPos
    },
  },
  created: function() {
    // const URL = "http://172.16.45.236:5000/seed";
    // this.images = this.get_data(URL, {hello: "world!"})
    let sampleImages = this.get_data_temp()
    this.images = sampleImages.slice(0,this.noStartImages)
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  position: relative;
  margin-top: 0;
}

#gui_container {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  z-index: 10000;
  display: flex;
  justify-content: flex-end;
}

h1 {
  font-family: "Avenir", Helvetica, sans-serif;
  text-size: 22px;
  text-align: center;
  color: black;
  margin: 30px 0 30px 0;
  line-height: 36px;
}

p {
  margin: 0;
  text-align: left;
  text-size: 22px;
}

#gui {
  padding: 0 35px;
  background-color: white;
}

#images_container {
  position: relative;
  z-index: 1;
}

body{
  background-color: #222;
  margin: 0;
  overflow: hidden;
} 

</style>
