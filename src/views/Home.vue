<template>
  <div class="home">
      <div class="container">
          <div class="home__content">
            <sign-up></sign-up>
          </div>
      </div>
  </div>
</template>

<script>
import SignUp from "../components/SignUp";
export default {
    components: {SignUp}
}
</script>

<style>
.home__content {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    width: 100vw;
    height: 100vh;
}
</style>



