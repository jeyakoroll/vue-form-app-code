<template>
  <section class="table__data">
    <table>
      <tr
        v-for="(el, idx) in fields"
        :key="idx"
      >
        <td>{{el.name}}</td>
        <td>{{el.value}}</td>
      </tr>
    </table>
  </section>
</template>

<script>
export default {
  props: {
    fields: {
      type: Array,
      required: true
    }
  }
}
</script>

<style lang="scss" scoped>

</style>


