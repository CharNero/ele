<template>
  <div class="ratingselect">
    <div class="rating-type border-1px">
      <span class="block positive" @click="select(2, $event)" :class="{'active': selectTypeBak === 2}">{{desc.all}}<em class="count">{{ratings.length}}</em></span>
      <span class="block positive" @click="select(0, $event)" :class="{'active': selectTypeBak === 0}">{{desc.positive}}<em class="count">{{positives.length}}</em></span>
      <span class="block negative" @click="select(1, $event)" :class="{'active': selectTypeBak === 1}">{{desc.negative}}<em class="count">{{negatives.length}}</em></span>
    </div>
    <div class="switch" @click="toggleContent" :class="{'on': onlyContentBak === true}">
      <span class="icon-check_circle"></span>
      <span class="text">只看有内容的评价</span>
    </div>
  </div>
</template>
<script>
const POSITIVE = 0
const NEGATIVE = 1
const ALL = 2
export default {
  props: {
    ratings: {
      type: Array,
      default () {
        return []
      }
    },
    selectType: {
      type: Number,
      default: ALL
    },
    onlyContent: {
      type: Boolean,
      default: false
    },
    desc: {
      type: Object,
      default () {
        return {
          all: '全部',
          positive: '满意',
          negative: '不满意'
        }
      }
    }
  },
  data () {
    return {
      selectTypeBak: this.selectType,
      onlyContentBak: this.onlyContent
    }
  },
  watch: {
    selectType (val) {
      this.selectTypeBak = val
    },
    selectTypeBak (val) {
      this.$emit('selectTypeChange', val)
    },
    onlyContent (val) {
      this.onlyContentBak = val
    },
    onlyContentBak (val) {
      this.$emit('onlyContentChange', val)
    }
  },
  methods: {
    select (type, e) {
      if (!e._constructed) {
        return
      }
      this.selectTypeBak = type
    },
    toggleContent (e) {
      if (!e._constructed) {
        return
      }
      this.onlyContentBak = !this.onlyContentBak
    }
  },
  computed: {
    positives () {
      return this.ratings.filter((rating) => {
        return rating.rateType === POSITIVE
      })
    },
    negatives () {
      return this.ratings.filter((rating) => {
        return rating.rateType === NEGATIVE
      })
    }
  }
}
</script>
<style lang="scss">
.ratingselect{
  .rating-type{
    padding: 18px 0;
    margin: 0 18px;
    font-size: 0;
    &::after{
      border-color: rgba(7, 17, 27, .1);
    }
    .block{
      display: inline-block;
      padding: 8px 12px;
      margin-right: 8px;
      border-radius: 2px;
      font-size: 12px;
      line-height: 16px;
      color: rgb(77, 85, 93);
      &.active{
        color: #fff;
      }
      &.positive{
        background: rgba(0, 160, 220, .2);
        &.active{
          background: rgb(0, 160, 220);
        }
      }
      &.negative{
        background: rgba(70, 85, 93, .2);
        &.active{
          background: rgb(70, 85, 93);
        }
      }
      .count{
        margin-left: 2px;
        font-size: 8px;
        font-style: normal;
      }
    }
  }
  .switch{
    padding: 12px 18px;
    line-height: 24px;
    font-size: 12px;
    color: rgb(147, 153, 159);
    border-bottom: 1px solid rgba(7, 17, 27, .1);
    font-size: 0;
    &.on .icon-check_circle{
      color: #00c850;
    }
    .icon-check_circle{
      display: inline-block;
      vertical-align: top;
      margin-right: 4px;
      font-size: 24px;
    }
    .text{
      display: inline-block;
      vertical-align: top;
      font-size: 12px;
    }
  }
}
</style>
