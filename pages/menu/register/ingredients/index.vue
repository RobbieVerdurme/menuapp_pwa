<template>
  <editIngredients :menu="menu" />
</template>

<script>
export default {
  middleware: 'authenticated',
  components: {
    editIngredients: () => import('~/components/organisms/registermenu-ingredients')
  },
  props: {
    menu: {
      type: Object,
      required: false,
      default: () => {}
    }
  }

}
</script>
