<template>
  <div class='singer'>
    <list-view
    @select="selectSinger"
    :data="singers"
    ref="list"
    ></list-view>
   <router-view></router-view>
  </div>
</template>

<script type='text/ecmascript-6'>
  import ListView from 'base/listview/listview'
  import { getSingerList } from 'api/singer'
  import { ERR_OK } from 'api/config'
  import Singer from 'common/js/singer'
  import { mapMutations } from 'vuex'
  import { playlistMixin } from 'common/js/mixin'

const HOT_SINGER_LEN = 10
const HOT_NAME = '热门'

export default {
  mixins: [playlistMixin],
  components: {
    ListView
  },
  data() {
    return {
      singers: []
    }
  },
  created() {
    this._getSingerList()
  },
  methods: {
    // handlePlaylist(playlist)   {
    //   const bottom = playlist.length > 0 ? '60px' : ''
    //   this.$refs.singer.style.bottom = bottom
    //   this.$refs.list.refresh()
    // },
    selectSinger(singer) {
      console.log(singer)
      this.$router.push({
        path: `/singer/${singer.id}`
      })
      console.warn('设置set')
      this.setSinger(singer)
    },
    _getSingerList() {
      getSingerList().then(res => {
        console.log(res.data.list)
        if (res.code === ERR_OK) {
          this.singers = this._normalizeSinger(res.data.list)
        }
      })
    },
    _normalizeSinger(list) {
      let map = {
        hot: {
          title: HOT_NAME,
          items: []
        }
      }
      list.forEach((item, index) => {
        if (index < HOT_SINGER_LEN) {
          map.hot.items.push(
            new Singer({
              name: item.Fsinger_name,
              id: item.Fsinger_mid
            })
          )
        }
        const key = item.Findex
        if (!map[key]) {
          map[key] = {
            title: key,
            items: []
          }
        }
        map[key].items.push(
          new Singer({
            name: item.Fsinger_name,
            id: item.Fsinger_mid
          })
        )
      })
      // 为了得到有序列表 处理数据map
      let ret = []
      let hot = []
      for (let key in map) {
        let val = map[key]
        // match() 方法将检索字符串 stringObject，以找到一个或多个与 regexp 匹配的文本
        if (val.title.match(/[a-zA-Z]/)) {
          ret.push(val)
        } else if (val.title === HOT_NAME) {
          hot.push(val)
        }
      }
      ret.sort((a, b) => {
        return a.title.charCodeAt(0) - b.title.charCodeAt(0)
      })
      // concat 连接两个数组 热门 + 所有
      return hot.concat(ret)
    },
    ...mapMutations({
      setSinger: 'SET_SINGER'
    })
  }
}
</script>

<style scope lang='scss'>
.singer {
  position: fixed;
  top: 88px;
  bottom: 0;
  width: 100%;
}
</style>
