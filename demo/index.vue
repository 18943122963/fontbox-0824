<template>
  <demo-section>
    <demo-block title="长文字容器">
      <dfaceFontbox :str="str" :config="config" />
      <dfaceFontbox :str="str2" :config="config" />
    </demo-block>
  </demo-section>
</template>

<script>
import dfaceFontbox from '../index';

export default {
  components: {
    dfaceFontbox,
  },
  data() {
    return {
      str: '12345',
      str2: '1234',
      config: {
        range: [
          ['min', 5, 50],
          [5, 'max', 25],
        ],
      },
    };
  },
};
</script>
