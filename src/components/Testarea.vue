<template>
  <textarea
    class="testarea"
    name="testarea"
    id="testarea"
    cols="45"
    rows="2"
    placeholder="Focus this box and begin typing to test"
    ref="testarea"
    :value="testText"
    @input="updateTestText"
    @keydown="detectKey"
    @keyup="releaseKey" />
</template>

<script>
export default {
  name: "testarea",
  mounted() {
    this.$refs.testarea.focus();
  },
  methods: {
    detectKey(e) {
      // eslint-disable-next-line no-console
      console.log(e.code);
      if (
        e.code === "Tab" ||
        e.code === "F5" ||
        e.code === "ContextMenu" ||
        e.code === "OSLeft" ||
        e.code === "MetaLeft"
      ) {
        e.preventDefault();
      }
      this.$store.commit("setActive", e.code);
      this.$store.commit("pressKey", e.code);
    },
    releaseKey(e) {
      this.$store.commit("releaseKey", e.code);
    },
    updateTestText(e) {
      this.$store.commit("updateTestText", e.target.value);
    }
  },
  computed: {
    testText() {
      const { testText } = this.$store.state;
      if (this.$refs.testarea !== undefined && testText === "") {
        this.$refs.testarea.focus();
      }
      return testText;
    }
  }
};
</script>

<style>
.testarea {
  border: 2px solid steelblue;
  border-top-left-radius: 5px;
  border-bottom-left-radius: 5px;
  margin-right: 1px;
  padding: 0.25rem;
}
</style>
