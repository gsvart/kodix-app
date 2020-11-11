<template>
    <div id="app">
        <div class="main-wrap">
            <button @click="changeVisibility" class="btn-skeleton">Досрочный платёж</button>
            <transition name="fade">
                <Modal v-if="showModal" @hide="changeVisibility" />
            </transition>
        </div>
    </div>
</template>

<script>
import Modal from './components/Modal'

export default {
  name: 'App',
  components: {
    Modal
  },
  data() {
    return {
      showModal: false
    }
  },
  methods: {
    changeVisibility() {
      return this.showModal = !this.showModal;
    }
  }
}
</script>

<style lang="scss">
@import "./assets/styles/main.css";

.main-wrap {
  position: relative;

  height: 100%;

  background: -webkit-linear-gradient(194.65deg, #dc3131 0.83%, rgba(255, 79, 79, 0) 108.93%), #ff5e56;
  background:      -o-linear-gradient(194.65deg, #dc3131 0.83%, rgba(255, 79, 79, 0) 108.93%), #ff5e56;
  background:         linear-gradient(255.35deg, #dc3131 0.83%, rgba(255, 79, 79, 0) 108.93%), #ff5e56;
}

.btn-skeleton {
  position: absolute;
  top: 50%;
  left: 50%;

  display: block;
  width: 210px;
  height: 56px;
  padding: 0 10px;

  font-weight: 500;
  font-size: 16px;
  line-height: 100%;
  font-family: inherit;
  text-align: center;
  color: #ffffff;

  background: transparent;
  border: 1px solid #ffffff;
  border-radius: 6px;
  outline: none;
  box-shadow: none;
  transform: translate(-50%, -50%);
  cursor: pointer;

  transition: 0.3s;

  &:hover,
  &:active {
    color: #000000;

    background: #ffffff;
  }

  &:disabled,
  &:disabled:hover {
    color: #ffffff;

    background: #bec5cc;
    border: none;
  }
}

</style>
