<template>
  <transition name="slide">
   <music-list
    :songs="songs"
    :title="title"
    :bg-image="bgImage"
   ></music-list>
  </transition>


</template>

<script type="text/ecmascript-6">
  import MusicList from '../music-list/music-list'
  import {getSingerDetail} from '../../api/singer'
  import {ERR_OK} from 'api/config'
  import {createSong} from 'common/js/song'
  import {mapGetters} from 'vuex'

  export default {
  computed:{
    title(){
      return this.singer.name
    },
    bgImage(){
      return this.singer.avatar
    },

    //得到vuex 中的数据
    ...mapGetters([
      'singer'
    ]),

  },
    created(){
      console.log(this.singer.name);
      this._getDetail()
    },
    data(){
      return{
        songs:[]
      }
    },
    methods:{
    _getDetail(){
      if(!this.singer.id){
        this.$router.push('/singer')
        return
      }
      getSingerDetail(this.singer.id).then((res)=>{
        console.log(res)
       if(res.code===ERR_OK){
         // this.songs=res.data.list
        this.songs=this._normalizeSongs(res.data.list)
       }
      })
    },
      _normalizeSongs(list){
        let ret=[]
        list.forEach((item)=>{

          let {musicData}=item
          if(musicData.songid&&musicData.albummid){
            ret.push(createSong(musicData))
          }

        })
        return ret
      }
    },
    components:{
      MusicList
    }
    // computed: {
    //   title() {
    //     return this.singer.name
    //   },
    //   bgImage() {
    //     return this.singer.avatar
    //   },
    //   ...mapGetters([
    //     'singer'
    //   ])
    // },
    // data() {
    //   return {
    //     songs: []
    //   }
    // },
    // created() {
    //
    // },
    // methods: {
    //   _getDetail() {
    //     if (!this.singer.id) {
    //       this.$router.push('/singer')
    //       return
    //     }
    //     getSingerDetail(this.singer.id).then((res) => {
    //       if (res.code === ERR_OK) {
    //         this.songs = this._normalizeSongs(res.data.list)
    //       }
    //     })
    //   },
    //   _normalizeSongs(list) {
    //     let ret = []
    //     list.forEach((item) => {
    //       let {musicData} = item
    //       if (musicData.songid && musicData.albummid) {
    //         ret.push(createSong(musicData))
    //       }
    //     })
    //     return ret
    //   }
    // },
    // components: {
    //   MusicList
    // }
  }
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/variable"

</style>
