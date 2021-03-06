<template>
  <div class="header">
    <div class="content-wrap">
      <div class="avatar">
        <img :src="seller.avatar" width="64" height="64" alt="">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div v-if="seller.supports" class="support">
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div v-if="seller.supports" class="support-count" @click="showDetail">
        <span class="count">{{seller.supports.length}}个</span>
        <i class="icon-keyboard_arrow_right"></i>
      </div>
    </div>
    <div class="bulletin-wrap" @click="showDetail">
      <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
      <i class="icon-keyboard_arrow_right"></i>
    </div>
    <div class="background">
      <img :src="seller.avatar" width="100%" height="100%" alt="">
    </div>
    <transition name="fade">
      <div class="detail" v-show="detailShow">
        <div class="detail-wrap clearfix">
          <div class="detail-main">
            <h1 class="name">{{seller.name}}</h1>
            <div class="star-wrap">
              <star :size="48" :score="seller.score"></star>
            </div>
            <div class="title">
              <div class="line"></div>
              <div class="text">优惠信息</div>
              <div class="line"></div>
            </div>
            <ul v-if="seller.supports" class="supports">
              <li class="support-item" v-for="(item,index) in seller.supports" :key="index">
                <span class="icon" :class="classMap[seller.supports[index].type]"></span>
                <span class="text">{{seller.supports[index].description}}</span>
              </li>
            </ul>
            <div class="title">
              <div class="line"></div>
              <div class="text">商家公告</div>
              <div class="line"></div>
            </div>
            <div class="bulletin">
              <p class="content">{{seller.bulletin}}</p>
            </div>
          </div>
        </div>
        <div class="detail-close" @click="hideDetail">
          <i class="icon-close">✖</i>
        </div>
      </div>
    </transition>
  </div>
</template>
<script>
import star from 'components/star/star'
export default {
  props: {
    seller: {
      type: Object
    }
  },
  data () {
    return {
      detailShow: false
    }
  },
  created () {
    this.classMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special']
  },
  methods: {
    showDetail () {
      this.detailShow = true
    },
    hideDetail () {
      this.detailShow = false
    }
  },
  components: {
    star
  }
}
</script>
<style lang="scss">
.header{
  position: relative;
  color: #fff;
  overflow: hidden;
  background-color: rgba(7, 17, 27, .5);
  .content-wrap{
    position: relative;
    padding: 24px 12px 18px 24px;
    font-size: 0;
    .avatar{
      display: inline-block;
      vertical-align: top;
      img{
        border-radius: 2px;
      }
    }
    .content{
      display: inline-block;
      margin-left: 16px;
      .title{
        margin: 2px 0 8px;
        .brand{
          display: inline-block;
          width: 30px;
          height: 18px;
          vertical-align: top;
          background-image: url('brand@2x.png');
          background-size: 30px 18px;
          background-repeat: no-repeat;
        }
        .name{
          margin-left: 6px;
          font-size: 16px;
          line-height: 18px;
          font-weight: bold;
        }
      }
      .description{
        margin-bottom: 10px;
        line-height: 12px;
        font-size: 12px;
      }
      .support .icon{
        display: inline-block;
        width: 12px;
        height: 12px;
        margin-right: 4px;
        vertical-align: top;
        background-repeat: no-repeat;
        background-size: 12px 12px;
        &.decrease{
          background-image: url("decrease_1@2x.png");
        }
        &.discount{
          background-image: url("discount_1@2x.png");
        }
        &.guarantee{
          background-image: url("guarantee_1@2x.png");
        }
        &.invoice{
          background-image: url("invoice_1@2x.png");
        }
        &.special{
          background-image: url("special_1@2x.png");
        }
      }
      .support .text{
        line-height: 12px;
        font-size: 10px;
      }
    }
    .support-count{
      position: absolute;
      right: 12px;
      bottom: 18px;
      padding: 0 8px;
      height: 24px;
      line-height: 24px;
      border-radius: 14px;
      background: rgba(0, 0, 0, .2);
      text-align: center;
      .count{
        font-size: 10px;
        vertical-align: top;
      }
      .icon-keyboard_arrow_right{
        margin-left: 2px;
        font-size: 10px;
        line-height: 24px;
      }
    }
  }
  .bulletin-wrap{
    position: relative;
    height: 28px;
    line-height: 28px;
    padding: 0 22px 0 12px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    background: rgba(7, 17, 27, .2);
    .bulletin-title{
      display: inline-block;
      width: 22px;
      height: 12px;
      margin-top: 8px;
      vertical-align: top;
      background: url("bulletin@2x.png") no-repeat;
      background-size:  22px 12px;
    }
    .bulletin-text{
      margin: 0 4px;
      vertical-align: top;
      font-size: 10px;
    }
    .icon-keyboard_arrow_right{
      position: absolute;
      right: 12px;
      top: 8px;
      font-size: 10px;
    }
  }
  .background{
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: -1;
    filter: blur(10px);
  }
  .detail{
    position: fixed;
    top: 0;
    left: 0;
    z-index: 100;
    width: 100%;
    height: 100%;
    overflow: auto;
    transition: all .5s;
    backdrop-filter: blur(10px);
    background: rgba(7, 17, 27, .8);
    &.fade-enter-active,&.fade-leave-active{
      opacity: 1;
      background: rgba(7, 17, 27, .8);
    }
    &.fade-enter, &.fade-leave-to{
      opacity: 0;
      background: rgba(7, 17, 27, 0);
    }
    .detail-wrap{
      min-height: 100%;
      width: 100%;
      .detail-main{
        margin-top: 64px;
        padding-bottom: 64px;
        .name{
          line-height: 16px;
          text-align: center;
          font-size: 16px;
          font-weight: 700;
        }
        .star-wrap{
          margin-top: 18px;
          padding: 2px 0;
          text-align: center;
        }
        .title{
          display: flex;
          width: 80%;
          margin: 28px auto 24px;
          .line{
            flex: 1;
            position: relative;
            top: -6px;
            border-bottom: 1px solid rgba(255, 255, 255,.2)
          }
          .text{
            padding: 0 12px;
            font-size: 14px;
            font-weight: 700
          }
        }
        .supports{
          width: 80%;
          margin: 0 auto;
          .support-item{
            padding: 0 12px;
            margin-bottom: 12px;
            font-size: 0;
            &:last-child{
              margin-bottom: 0;
            }
            .icon{
              display: inline-block;
              width: 16px;
              height: 16px;
              vertical-align: top;
              margin-right: 6px;
              background-size: 16px 16px;
              background-repeat: no-repeat;
              &.decrease{
                background-image: url("decrease_1@2x.png");
              }
              &.discount{
                background-image: url("discount_1@2x.png");
              }
              &.guarantee{
                background-image: url("guarantee_1@2x.png");
              }
              &.invoice{
                background-image: url("invoice_1@2x.png");
              }
              &.special{
                background-image: url("special_1@2x.png");
              }
            }
            .text{
              line-height: 16px;
              font-size: 12px;
              vertical-align: top;
            }
          }
        }
        .bulletin{
          width: 80%;
          margin: 0 auto;
          .content{
            padding: 0 12px;
            line-height: 24px;
            font-size: 12px;
          }
        }
      }
    }
    .detail-close{
      position: relative;
      width: 32px;
      height: 32px;
      margin: -64px auto 0;
      clear: both;
      font-size: 24px;
      .icon-close{
        font-style: normal;
      }
    }
  }
}
</style>
