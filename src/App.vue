<template>
  <div id="app">
    <router-view></router-view>
  </div>
</template>

<style>
:root {
  --color-primary: #08d;
  --color-primary-light: #303245;
  --color-primary-dark: #15172b;

  --color-primary-grey: #808080;

  --color-danger: #FF0000;

  --bg-color: #000;

  --br-small: 1.2rem;
  --br-big: 0.6rem;
}

* {
  margin: 0;
  padding: 0;
}

*,
*::before,
*::after {
  box-sizing: inherit;
}

html {
  box-sizing: border-box;
  font-size: 1rem;
}

body {
  font-family: 'Open Sans', sans-serif;
  font-weight: 400;
  line-height: 1.6;
  color: #fff;
  min-height: 100vh;
  width: 100vw;
  background-color: var(--bg-color) ;
}

.container {
  max-width: 120rem;
  margin: 0 auto;
}

ul {
  list-style: none;
}

a {
  text-decoration: none;
  color: inherit;
}

.heading-primary {
  font-size: 2rem;
}

.heading-ternary {
  font-size: 1rem;
}
</style>
