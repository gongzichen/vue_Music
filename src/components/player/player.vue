<template>
    <div class="player" v-show="playlist.length>0">
        <transition name='normal'
                    @enter="enter"
                    @after-enter="afterEnter"
                    @leave="leave"
                    @after-leave="afterLeave"
        >
        <div class="normal-player"  v-show="fullScreen">
           <div class="background">
               <img :src="currentSong.image" height="100%" width="100%">
           </div>
           <div class="top">
               <div class="back" @click="back">
                   <i class="icon-back"></i>
               </div>
               <h1 class="title" v-html="currentSong.name"></h1>
               <h2 class="subtitle" v-html="currentSong.singer"></h2>
           </div>
           <div class="middle"></div>
        </div>
        </transition>
    </div>
</template>
