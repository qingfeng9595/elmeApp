<template>
    <div class="ratingselect">
        <div class="rating-type">
           <span @click="select(2,$event)" class="block positive" :class="{'active':selectType ==2}">
               {{desc.all}}
               <span class="count">{{ratings.length}}</span>
            </span>
           <span @click="select(0,$event)" class="block positive" :class="{'active':selectType ==0}">
               {{desc.positive}}
               <span class="count">{{positive.length}}</span>
            </span>
           <span @click="select(1,$event)" class="block negative" :class="{'active':selectType ==1}">
               {{desc.negative}}
               <span class="count">{{negative.length}}</span>
            </span>
        </div>
        <div @click="toggleContent" class="switch" :class="{'on':onlyContent}">
            <span class="icon-check_circle"></span>
            <span class="text">只看有内容的评价</span>
        </div>
    </div>
</template>

<script>
const POSITIVE = 0;
const NEGATIVE = 1;
const ALL = 2;

export default {
  props: {
    ratings: {
      type: Array,
      default () {
        return [];
      }
    },
    selectTypeProps: {
      type: Number,
      default: ALL
    },
    onlyContentProps: {
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
        };
      }
    }
  },
  data () {
    return {
      selectType: this.selectTypeProps,
      onlyContent: this.onlyContentProps
    };
  },
  computed: {
    positive () {
      return this.ratings.filter((rating) => {
        return rating.rateType === POSITIVE;
      });
    },
    negative () {
      return this.ratings.filter((rating) => {
        return rating.rateType === NEGATIVE;
      });
    }
  },
  methods: {
    select (type, event) {
      if (!event._constructed) {
        return;
      }
      this.selectType = type;
      this.$emit('ratingtype-select', type);
    },
    toggleContent (event) {
      if (!event._constructed) {
        return;
      }
      this.onlyContent = !this.onlyContent;
      this.$emit('content-toggle', this.onlyContent);
    }
  }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import '../../common/stylus/mixin';
  .ratingselect
    .rating-type
      padding 18px 0
      margin 0 18px
      border-1px(rgba(7,17,27,0.1))
      font-size 0
      .block
        display inline-block
        padding 8px 12px
        margin-right 8px
        border-radius 1px
        font-size 12px
        color rgb(77,85,93)
        &.active
          color #fff
        .count
          font-size 8px
          margin-left 2px
          line-height 16px
        &.positive
          background rgba(0,160,220,0.2)
          &.active
            background rgb(0,160,220)
        &.negative
          background rgba(77,85,93,0.2)
          &.active
            background rgb(77,85,93)
    .switch
      padding 12px 18px
      line-height 24px
      border-bottm 1px solid rgba(0,160,220,0.1)
      color rgb(147,153,159)
      font-size 0
      &.on
        .icon-check_circle
          color #00c850
      .icon-check_circle
        display inline-block
        vertical-align top
        font-size 24px
        margin-right 4px
      .text
        font-size 12px
</style>
