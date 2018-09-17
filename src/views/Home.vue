<template>
<div class="page home">
  <div class="home__send">
    <div class="send__messages">
      <div class="message">
        <p class="message__text">You're terrible <span class="message__score">1</span></p>
        <p class="message__time">0.5s</p>
      </div>

    </div>
    <form class="send__form" @submit.prevent>
      <input type="text" name="message" class="send__form__message"  placeholder="Say something" autocomplete="off">
      <button class="send__form__submit" type="submit">Send</button>
    </form>
  </div>
  <div class="home__dash">
    <p class="dash__score yellow">50</p>
    <div class="dash__slider">
      <div class="slider__description">
        <p>positive</p>
      <p>negative</p>
      </div>
      
      <div class="sliderContainer">
        <input class="slider" type="range" min="1" max="100" value="50" disabled>
      </div>
    </div>
    <p class="dash__stat">computational time: 0.5s</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'home'
}
</script>
