<script setup lang="ts">
import { onMounted, ref } from 'vue';
import gsap from 'gsap'
import {ScrollTrigger} from 'gsap/ScrollTrigger'
import BgVideo from './assets/bg-video.mp4'

gsap.registerPlugin(ScrollTrigger)

const content = ref(null)
const vid = ref(null)

// basically used this as my reference:
// https://codepen.io/shshaw/pen/vYKBPbv/9e810322d70c306de2d18237d0cb2d78

onMounted(() => {

    // handles iOS play issue
    once(document.documentElement, "touchstart", function (e) {
        vid.value.play()
        vid.value.pause()
    })
    
    // setup scrollTrigger
    let tl = gsap.timeline({
        defaults: {duration: 1},

        scrollTrigger: {
            trigger: content.value,
            start: 'top top',
            end: 'bottom bottom',
            scrub: true
        }
    })

    // link video time to animation timeline
    once(vid.value, "loadedmetadata", () => {
        tl.fromTo(vid.value, 
        {currentTime: 0}, 
        {currentTime: vid.value.duration || 1})
    })
})

// cool helper function triggering an event only one time...
// although I wonder why they didn't use the "once" param:
// @link: https://developer.mozilla.org/en-US/docs/web/api/eventtarget/addeventlistener#parameters
function once (el, event, func, opts = {}) {
    var onceFn = function (e: Event) {
        el.removeEventListener(event, onceFn)
        func.apply(this, arguments)
    }

    el.addEventListener(event, onceFn, opts)
    return onceFn
}

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