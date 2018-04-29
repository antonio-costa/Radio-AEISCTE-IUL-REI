<template>
	<div id="radioplayer">
		<div class="wrapper">
			<div class="album-cover" v-bind:style="{'background-image': 'url('+dnowPlaying.art+')'}">
			</div>

			<div class="song-info">
				<div class="artist">
					{{dnowPlaying.artist}}
				</div>
				<div class="title">
					{{dnowPlaying.title}}
				</div>
				<div class="toggle-play" :class="{'disabled': disabled}" v-on:click="togglePlayback">
					<div class="toggle-symbol" :class="{paused: !isPlaying}"></div>
				</div>
			</div>
		</div>
		<audio id="rpAudio" :src="audioSource" type="audio/mpeg" :autoplay="autoplay"></audio>
	</div>
</template>


<script>

const UNKOWN_ARTIST = 'Artista desconhecido';
const UNKOWN_TITLE = 'Sem título';
const NO_ART = 'https://4vector.com/i/free-vector-cd-icon_101662_CD_icon.png';

export default {
	name: 'radio-player',
	props:{
		noArt: {
			default: NO_ART,
			type: String
		},
		audioSource: {
			default: '',
			type: String
		},
		play: {
			default: true,
			type: Boolean
		},
		nowPlaying: {
			type: Object
		},
		disabled: {
			default: false,
			type: Boolean
		}
	},
	data: function() {
		return {
			autoplay: this.play,
			isPlaying: this.play,
			dnowPlaying: this.nowPlaying,
		};
	},
	methods: {
		togglePlayback: function() {
			if(this.disabled) return;
			// update playback data
			this.isPlaying = !this.isPlaying;
		}
	},
	watch: {
		// plays/stops audio whenever isPlaying is changed
		isPlaying: function() {
			if(this.isPlaying) document.getElementById("rpAudio").load();
			else document.getElementById("rpAudio").pause();

			// send event to parent
			this.$emit('update:play', this.isPlaying);
		},
		play: function(newValue) {
			this.isPlaying = newValue;
		},
		nowPlaying: {
			immediate: true,
			handler: function(newValue, oldValue) {
				this.dnowPlaying = {
					title: newValue.title || UNKOWN_TITLE,
					artist: newValue.artist || UNKOWN_ARTIST,
					art: newValue.art || this.noArt,
				};
			}
		}
	}
}
</script>


<style scoped>
.radioplayer {
	display: inline-block;
	overflow: hidden;
	width: 100%;
	height: 100%;
}
.wrapper {
	display: flex;
	justify-content: center;
	align-items: center;

	width: 100%;
	height: 100%;
}
.album-cover {
	box-shadow: 0px 0px 40px rgba(0,0,0,0.5);

	width: 160px;
	height: 160px;


	background-position: center center;
	background-size: cover;
	background-color: #232323;
	background-image: url(https://4vector.com/i/free-vector-cd-icon_101662_CD_icon.png);
}
.song-info {
	flex: 1;
	margin-left: 20px;
	text-align: left;

}
.artist {
	font-size: 0.8rem;
	color: darkgrey;
	margin-bottom: 6px;
}
.title {
	font-size: 1.4rem;
	margin-bottom: 20px;
	color: white;
}
.toggle-play  {
	width: 50px;
	height: 50px;
	background-color: royalblue;
	border-radius: 100px;

	display: flex;
	justify-content: center;
	align-items: center;

	cursor: pointer;

	transition: 0.5s all;
}
.toggle-play.disabled {
	background-color: #c1c1c1;
}
.toggle-symbol {
	margin-left: 0;
	border-style: double;
	border-width: 0px 0 0px 20px;
	border-color: transparent transparent transparent white;
	width: 20px;
	height: 20px;

	box-sizing: border-box;

	transition: 100ms all ease;
}
.toggle-symbol.paused {
	margin-left: 1px;
	border-style: solid;
	border-width: 10px 0px 10px 20px;
	
}
</style>