<template>
  <button
      @click.prevent="onClick"
      class="sc-user-input--send-icon-wrapper"
    >
      <svg class="sc-user-input--send-icon"
           xmlns="http://www.w3.org/2000/svg"
           enableBackground='new 0 0 37.393 37.393'
           viewBox="0 0 24 24">
          <path :style="{fill: color}" d="M12 14c1.66 0 2.99-1.34 2.99-3L15 5c0-1.66-1.34-3-3-3S9 3.34 9 5v6c0 1.66 1.34 3 3 3zm5.3-3c0 3-2.54 5.1-5.3 5.1S6.7 14 6.7 11H5c0 3.41 2.72 6.23 6 6.72V21h2v-3.28c3.28-.48 6-3.3 6-6.72h-1.7z"/>
          <path d="M0 0h24v24H0z" fill="none"/>
      </svg>
    </button>
</template>

<script>
export default {
  props: {
    onClick: {
      type: Function,
      required: true
    },
    color: {
      type: String,
      default: 'black'
    }
  }
}
</script>

<style scoped>
.sc-user-input--send-icon-wrapper {
  background: none;
  border: none;
  padding: 0px;
  margin: 0px;
  outline: none;
}

.sc-user-input--send-icon-wrapper:focus {
  outline: none;
}

.sc-user-input--send-icon {
  height: 20px;
  width: 20px;
  cursor: pointer;
  align-self: center;
  outline: none;
}

.sc-user-input--send-icon:hover path {
  filter: contrast(15%);
}
