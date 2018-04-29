<template>
<div class="last-played">
      <div class="header">&#9835 TOCOU</div>
      <transition-group name="last-played" tag="ul">
        <li v-for="item in lastPlayed" :key="item.id">
          <div class="artist">{{item.artist}}</div>
          <div class="title">{{item.title}}</div>
        </li>
      </transition-group>
    </div>
</template>

<script>
	export default {
		name: 'last-played-list',
		props: ['last-played'],
	}
</script>

<style scoped>
.last-played {
  width: 250px;
  text-align: left;
}
.last-played .header {
  font-size: 12px;
  font-weight: bold;
}
.last-played ul {
  list-style-type: none;
  margin: 0;
  padding: 0;
}
.last-played li {
  border-left: 2px solid #4286f4;
  padding-left: 6px;
  margin-top: 10px;
  text-shadow: 1px 1px 1px rgba(0,0,0,0.6);

  transition: 0.2s all;
}
.last-played li:hover {
  border-left: 5px solid #4286f4;
}
.last-played li .artist {
  font-size: 12px;
  color: #4286f4;
}

.last-played-item {
  transition: all 1s;
  display: inline-block;
  margin-right: 10px;
}
.last-played-enter, .last-played-leave-to
/* .list-complete-leave-active below version 2.1.8 */ {
  opacity: 0;
  transform: translateY(30px);
}
.last-played-leave-active {
  position: absolute;
}
</style>