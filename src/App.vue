<script setup lang="ts">
import BgVideo from './assets/bg-video.mp4'
import ScrollyVideo from 'scrolly-video/dist/ScrollyVideo.vue'

</script>

<template>
    <main class="h-[2000px]">
        <div class="fixed h-screen w-screen z-10 p-32">
            <h1 class="text-white text-9xl drop-shadow-sm">🥳</h1>
            <p class="text-white/75 text-xs font-bold uppercase tracking-widest absolute bottom-0 mb-32">[Scroll Up and Down]</p>
        </div>
        <ScrollyVideo :src="BgVideo" :debug="true" />
    </main>
</template>