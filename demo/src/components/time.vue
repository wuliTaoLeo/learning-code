<template>
  <p :style="{ color: color }">{{ time }}</p>
</template>

<script>
export default {
  props: {
    color: {
      type: String,
      default: "#D7000F"
    }
  },
  data() {
    return {
      time: 10
    };
  },
  mounted() {
    var t = setInterval(() => {
      this.time--;
      if (this.time === 0) {
        clearInterval(t);
        this.$emit("end");
      }
    }, 1000);
  }
};
</script>
