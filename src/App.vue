<template>
    <div id="app">
        <header class="bg-gray-900 text-white py-8">
            <h1 class="text-2xl flex items-center justify-center absolute right-20 top-6">BAD UX DESIGN</h1>
        </header>
        
        <main class="max-w-screen-md mx-auto px-4">
            <section class="player border border-red-500 m-2 flex flex-col items-center">
                <h2 class="song-title text-3xl font-bold">
                    {{ current.title }} - <span class="font-normal">{{ current.artist }}</span>
                </h2>

                <input type="range" min="0" :max="player.duration || 0" v-model="currentTime" @input="seekTo" class="w-full mt-4">

                <div class="controls flex items-center space-x-4 mt-4">
                    <button @click="adjustVolume('down')" class="volume-down text-white font-bold py-2 px-4 bg-blue-500 rounded">
                        Volume Down
                    </button>

                    <button @click="prev" class="prev text-white font-bold py-2 px-4 bg-red-500 rounded">
                        Prev
                    </button>

                    <button v-if="!isPlaying" @click="play" class="play text-white font-bold py-2 px-4 bg-pink-500 rounded">
                        Play
                    </button>

                    <button v-else @click="pause" class="pause text-white font-bold py-2 px-4 bg-pink-500 rounded">
                        Pause
                    </button>

                    <button @click="next" class="next text-white font-bold py-2 px-4 bg-red-500 rounded">
                        Next
                    </button>

                    <button @click="adjustVolume('up')" class="volume-up text-white font-bold py-2 px-4 bg-blue-500 rounded">
                        Volume Up
                    </button>
                </div>
            </section>

            <section class="playlist mt-4 border border-yellow-500">
                <h3 class="text-gray-800 text-2xl font-semibold mb-4">
                    The Playlist
                </h3>

                <button v-for="song in songs" :key="song.src" @click="play(song)" :class="['song', {'playing': song.src === current.src}]">
                    {{ song.title }} - {{ song.artist }}
                </button>
            </section>

            <!-- Horizontally aligned divs taking the excess height -->
            <div class="flex h-[280px] mt-4">
                <div class="flex-1 border border-blue-500 m-1 p-3">
                    <h2 class="strong">
                        Current Volume Level: {{ volumeLevel*10}}%
                    </h2>

                    <button class="mt-2 p-1 border border-black bg-gray-200" @click="randomChange">
                        Change
                    </button>
                </div>
                <div class="flex-1 border border-green-500 m-1 p-3">
                </div>
                <div class="flex-1 border border-purple-500 m-1 p-3"></div>
            </div>

        </main>
    </div>
</template>

<script>
export default {
name: 'app',
data() {
    return {
    current: {},
    index: 0,
    isPlaying: false,
    songs: [
        {
        title: 'NVMD',
        artist: 'ASDFSADFW',
        src: '/NVMD.mp3' // Adjust the path relative to the public directory
        }
    ],
    player: new Audio(),
    maxVolume: 1, // Max volume value
    minVolume: 0, // Min volume value
    volumeStep: 0.1, // Volume step for each adjustment
    volumeLevel: 10, // Initial volume level (example)
    currentTime: 0 // Current playback position
    };
},
methods: {
    play(song) {
    if (typeof song.src !== 'undefined') {
        this.current = song;
        this.player.src = this.current.src;
    }

    this.player.play();
    this.player.addEventListener('ended', () => {
        this.index++;
        if (this.index > this.songs.length - 1) {
        this.index = 0;
        }

        this.current = this.songs[this.index];
        this.play(this.current);
    });
    this.isPlaying = true;
    },
    pause() {
    this.player.pause();
    this.isPlaying = false;
    },
    next() {
    this.index++;
    if (this.index > this.songs.length - 1) {
        this.index = 0;
    }

    this.current = this.songs[this.index];
    this.play(this.current);
    },
    prev() {
    this.index--;
    if (this.index < 0) {
        this.index = this.songs.length - 1;
    }

    this.current = this.songs[this.index];
    this.play(this.current);
    },
    adjustVolume(direction) {
    let newVolume = this.player.volume;

    if (direction === 'up') {
        newVolume = Math.min(this.player.volume + this.volumeStep, this.maxVolume);
    } else if (direction === 'down') {
        newVolume = Math.max(this.player.volume - this.volumeStep, this.minVolume);
    }

    // Ensure newVolume is within valid range (0 to 1)
    if (newVolume >= this.minVolume && newVolume <= this.maxVolume) {
        this.player.volume = newVolume;
        this.volumeLevel = Math.round(newVolume * 10); // Example: Convert to 10-level scale
        console.log(`Volume adjusted to ${this.volumeLevel}/10`);
    } else {
        console.warn('Volume value out of range.');
    }
    },
    seekTo() {
    this.player.currentTime = this.currentTime;
    },
    randomChange() {
    const newVolume = Math.random() * (this.maxVolume - this.minVolume) + this.minVolume;

        // Ensure newVolume is within valid range (0 to 1)
        if (newVolume >= this.minVolume && newVolume <= this.maxVolume) {
            this.player.volume = newVolume;
            this.volumeLevel = Math.round(newVolume * 10); // Example: Convert to 10-level scale
            console.log(`Volume changed randomly to ${this.volumeLevel}/10`);
        } else {
            console.warn('Random volume value out of range.');
        }
    }
},
created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;

    // Update current time as audio progresses
    this.player.addEventListener('timeupdate', () => {
    this.currentTime = this.player.currentTime;
    });

    this.player.volume = .5; // Set initial volume (optional)
}
};
</script>