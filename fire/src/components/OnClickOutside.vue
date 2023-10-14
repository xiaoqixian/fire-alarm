<script>
export default {
  mounted(el, binding) {
    function eventHandler(e) {
      if (el.contains(e.target)) {
        return false
      }
      if (binding.value && typeof binding.value === 'function') {
        binding.value(e)
      }
    }

    //custom attributes for eventlistener closement before
    //this component is unmounted.
    el.__click_outside__ = eventHandler;

    document.addEventListener("click", eventHandler);
  },

  beforeUnmount(el) {
    document.removeEventListener('click', el.__click_outside__);
    delete el.__click_outside__;
  }
}
</script>
