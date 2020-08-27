<template>
  <div :style="divStyle">{{ strResult }}</div>
</template>

<script>
/* eslint-disable */
import { toArray } from './utils/toArray';
import { cutString } from 'dface-shared';
export default {
  name: 'dface-fontbox',
  props: {
    str: {
      require: true,
      type: String,
    },
    config: {
      type: Object,
      require: false,
      default: () => {},
    },
  },
  data() {
    return {
      divStyle: {},
      strResult: '',
    };
  },
  methods: {
    setString(config) {
      this.strResult = this.str;
      //获取长度
      const strLen = toArray(this.strResult).length;
      //若最大长度为空，则从范围域中取出最大值
      if (!config.max_len && config.range) {
        let max = 0;
        for (const item of config.range) {
          //若处理范围内出现了"max"，那么不需要进行最大长度截取
          if (item[1] === 'max') {
            max = Infinity;
            break;
          }
          //记录最大长度
          item[1] > max && (max = item[1]);
        }
        config.max_len = max;
      }
      //若未设定最小字体，那么取范围域中取出最小值
      if (!config.min_size && config.range) {
        let min = Infinity;
        for (const item of config.range) {
          //记录最小字体
          parseInt(item[2]) < min && (min = item[2]);
        }
        if (min === Infinity) min = 0;
        if (typeof min === 'number') min += 'px';
        config.min_size = min;
      }

      //若设定了最大长度且长度超标，那么截取字符串，并按最小字体来
      if (config.max_len && strLen >= config.max_len) {
        this.strResult = cutString(this.strResult, { cut_len: config.max_len });
        let temp = {};
        temp.fontSize = isNaN(config.min_size)
          ? config.min_size
          : config.min_size + 'px';
        this.divStyle = temp;
        return;
      }
      //读取配置，设定大小
      for (let item of config.range) {
        //item中，0为最小长度，1为最大长度，2为字体大小
        item[0] === 'min' && (item[0] = -Infinity);
        item[1] === 'max' && (item[1] = Infinity);
        if (strLen >= item[0] && strLen < item[1]) {
          let temp = {};
          temp.fontSize = isNaN(item[2]) ? item[2] : item[2] + 'px';
          this.divStyle = temp;
          break;
        }
      }
    },
  },
  watch: {
    //监听字符串长度
    str() {
      this.setString(this.config);
    },
  },
  mounted() {
    this.setString(this.config);
  },
};
</script>
<style scoped></style>
