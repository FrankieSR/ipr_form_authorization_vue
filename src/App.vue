<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Vue App</router-link>
    </div>
    <router-view/>
  </div>
</template>

<style lang="scss">
@import url('https://fonts.googleapis.com/css?family=Roboto');
body {
	margin: 0;
	padding: 0;
}

html {
	min-height: 100vh;
}

#app {
	font-family: 'Roboto', sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: #2c3e50;
	background: #eee;
	min-height: 100vh;
	padding-bottom: 50px;
}

#nav {
	padding: 30px;
	a {
		font-weight: bold;
		color: #2c3e50;
		&.router-link-exact-active {
			color: #42b983;
		}
	}
}
</style>
