<script setup lang="ts">
import { onMounted, ref } from 'vue';
import gsap from 'gsap'
import {ScrollTrigger} from 'gsap/ScrollTrigger'
import BgVideo from './assets/bg-video.mp4'
import type { Ref } from 'vue'

gsap.registerPlugin(ScrollTrigger)

const content: Ref<HTMLDivElement | null> = ref(null)
const vid:Ref<HTMLVideoElement | null> = ref(null)

// basically used this as my reference:
// https://codepen.io/shshaw/pen/vYKBPbv/9e810322d70c306de2d18237d0cb2d78

onMounted(() => {
    const video = vid.value as HTMLVideoElement

    // handles iOS play issue
    document.documentElement.addEventListener("touchstart", () => {
        video.play()
        video.pause()
    }, {once: true})
    
    // setup scrollTrigger
    let tl = gsap.timeline({
        defaults: {duration: 1},

        scrollTrigger: {
            trigger: content.value,
            start: '25%', // <-- this determines when the video begins to play
            end: 'bottom bottom',
            scrub: true,
            markers: true // <-- show markers for debugging
        }
    })

    // link video time to animation timeline
    video.addEventListener("loadedmetadata", () => {
        tl.fromTo(
            vid.value, 
            {currentTime: 0}, 
            {currentTime: video.duration || 1}
        ) 
    }, {once: true})
})
</script>

<template>
    <main class="h-screen">
        <div ref="content" class="relative z-10 h-[5000px] w-full p-32">
            <h1 class="text-white text-9xl drop-shadow-sm">🥳</h1>
            <p class="absolute bottom-0 mb-32 text-xs font-bold tracking-widest uppercase text-white/75">[Scroll Up and Down]</p>
        </div>

        <video :src="BgVideo" ref="vid" class="fixed top-0 left-0 object-cover w-full h-full" />
    </main>
</template>