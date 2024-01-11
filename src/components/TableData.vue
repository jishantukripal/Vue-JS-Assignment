<template>
    <td>
      <slot :data="data"></slot>
    </td>
</template>
  
<script>
  export default {
    props: {
      data: String,
    },
  };
</script>
  