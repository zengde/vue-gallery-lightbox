<template>
  <div id="app" style="background-color: yellow" class="row">
    <div>
      <ul>
        <li 
          v-for="(image, index) in images" 
          style="display: inline-block"
        >
          <img 
            v-lazy="image.thumb" 
            style="height: 100px" 
            @click="openGallery(index)"
          >
        </li>
      </ul>
      <LightBox 
	    v-if="showBox"
        :images="images" 
        ref="lightbox"
        :show-caption="true"
        :start-at="currentIndex"
      >
	  <template #imgcon="{data,index}">
         <Xgplayer :key="index" :config="play(data,index)" @player="Player = $event"/>
      </template>
	  </LightBox>
    </div>
  </div>
</template>

<script>
import LightBox from 'components/LightBox'

import siteLoading from './siteloading.gif'
import images from './dummy-video'
import Xgplayer from 'xgplayer-vue'
//import 'xgplayer-mp4'
//import FlvPlayer from 'xgplayer-flv';

export default {
  components: {
    LightBox,
	Xgplayer
  },

  data () {
    return {
      images,
      siteLoading,
	  config: {
		id: 'vs',
		autoplay: true,
		url:'',
		playbackRate: [0.5, 0.75, 1, 1.5, 2],
		download: true,
		screenShot: true,
		width: '100%',
		height: '100%'
	  },
	  Player: null,
	  showBox:false,
	  currentIndex:null
    }
  },

  methods: {
    openGallery(index) {
      this.currentIndex=index;
	  if(!this.showBox){
		this.showBox=true;
	  }else{
		this.$refs.lightbox.showImage(index);
	  }
    },
	play(data,index){
       return Object.assign(this.config,{url:data.src,poster:data.thumb});
    }
  }
}
</script>
