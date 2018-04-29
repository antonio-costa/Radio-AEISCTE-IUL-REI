<template>
	<div>
		<transition name="notification">
			<div id="connection-notification-bar" :class="{'offline': offline, 'online': !offline}" v-if="visible">{{message}}</div>
		</transition>
	</div>
</template>

<script>
import axios from 'axios';

export default {
	name: 'connection-listener',
	data: function() {
		return {
			offline: false,
			visible: false,
			timeout: null,
			message: 'A tua ligação foi perdida. Estás offline.',
			el: null
		}
	},
	created: function() {
		window.addEventListener('online', this.updateStatusOnline)
		window.addEventListener('offline', this.updateStatusOffline)
	},
	mounted: function() {
		this.el = document.getElementById('connection-notification-bar')
	},
	methods: {
		updateStatusOnline: function() {

			this.offline = false;
			this.message = 'Ligação reestabelecida.';

			let that = this;
			clearTimeout(this.timeout)
			this.timeout = setTimeout(function() {
				that.visible = false;
			}, 2*1000)

			this.$emit('update:connection-status', true)
		},
		updateStatusOffline: function() {
			this.offline = true;
			this.visible = true;
			clearTimeout(this.timeout)
			this.message = 'A tua ligação foi perdida. Estás offline.';
			this.$emit('update:connection-status', false)
		},
	}
}
</script>

<style scoped>
#connection-notification-bar {
	z-index: 999999;
	position: fixed;
	top: 0;
	left: 0;
	right: 0;
	height: 20px;

	display: flex;
	justify-content: center;
	align-items: center;

	color: white;
	font-size: 14px;
	text-shadow: 1px 1px 1px rgba(0,0,0,0.5);

	transition: 0.5s all ease;
}
.notification-enter-active {
	transition: opacity 5s;
	background-color: #f26154;
}
.notification-enter {
	background-color: transparent;
	opacity: 0;
}
.notification-leave-active {
	transition-delay: 2s;
	opacity: 0;
	background-color: #52e830;
}
#connection-notification-bar.online {
	background-color: #52e830;
}
#connection-notification-bar.verifying {
	background-color: #efb923;
}
#connection-notification-bar.offline {
	background-color: #f26154;
}
</style>