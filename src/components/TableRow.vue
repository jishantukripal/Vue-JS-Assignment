<template>
    <tr>
      <slot :rowData="rowData"></slot>
    </tr>
</template>

<script>
  export default {
    props: {
      rowData: Object,
    },
  };
</script>
  