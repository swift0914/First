<template>
  <div class='mine-trade-container'>
    <div class='mine-trade-item' @click='buttonClick(1)'>
      <div class='quantity-container'>
        <span class='sum-rmb'>¥</span>
        <span class='quantity-amount'>25.63</span>
        <span class='unit'>万</span>
      </div>
      <div class='illustrate-container'>
        <p>月总收入</p>
        <img class='next-icon'
             src='https://img.goshare2.com/app-images/h5/mine-tread-next-icon.png' />
      </div>
    </div>
    <div class='mine-trade-item' @click='buttonClick(2)'>
      <div class='quantity-container'>
        <span class='quantity-amount'>90</span>
        <span class='unit'>件</span>
      </div>
      <div class='illustrate-container'>
        <p>月售出数</p>
        <img class='next-icon'
             src='https://img.goshare2.com/app-images/h5/mine-tread-next-icon.png' />
      </div>
    </div>
    <div class='mine-trade-item'>
      <p class='launched'>待上线</p>
      <div class='illustrate-container'>
        <p>账号服务分</p>
        <img class='next-icon'
             src='https://img.goshare2.com/app-images/h5/mine-tread-next-icon.png' />
      </div>
    </div>
    <div class='mine-trade-item' @click='buttonClick(4)'>
      <div class='purse-container'>
        <span class='rmb'>¥</span>
        <span class='balance' :class='{ close: !balanceHint }'>{{ getBalanceValue }}</span>
        <span class='unit' v-if='balanceHint && isWalletInWan'>{{ getWalletUnit }}</span>
        <img :src='getBalanceHintIcon()' class='balance-icon' @click.stop='openHintBalance()' />
      </div>
      <div class='illustrate-container'>
        <p>钱包余额</p>
      </div>
    </div>
  </div>
</template>
<script>

import { navigate } from '@goshare2/router';
import environment from '@/environments/environment';

export default {
  name: 'mine-business-trade',
  props: {
    userInfo: {
      typeof: Object,
      default: () => null,
    },
  },
  data() {
    return {
      balanceHint: false,
    };
  },
  computed: {
    getBalanceValue() {
      return this.balanceHint && this.userInfo ? this.formatNumber(this.userInfo.wallet) : '****';
    },
    isWalletInWan() {
      if (!this.userInfo) return false;
      const num = Number(this.userInfo.wallet);
      if (this.$appLanguage.isEnglish) {
        return num >= 1000;
      }
      return num >= 10000;
    },
    getWalletUnit() {
      if (this.$appLanguage.isEnglish) {
        return this.$appLanguage.value('K', 'K');
      }
      return this.$appLanguage.value('万', '万');
    },
  },
  methods: {
    buttonClick(type) {
      if (type === 1) {
        navigate.navigateTo('opennewweb', {
          url: encodeURIComponent(
            `${environment.MIX_HOST}/wv/mine/occupation/revenue/?hideHeader=1`,
          ),
        });
      } else if (type === 2) {
        navigate.navigateTo('opennewweb', {
          url: encodeURIComponent(
            `${environment.MIX_HOST}/wv/mine/occupation/monthly-sell/?hideHeader=1`,
          ),
        });
      } else if (type === 4) {
        this.$emit('walletClick');
      }
    },
    openHintBalance() {
      this.balanceHint = !this.balanceHint;
    },
    getBalanceHintIcon() {
      return this.balanceHint
        ? 'https://img.goshare2.com/app-images/h5/mine-black-open-eye.png'
        : 'https://img.goshare2.com/app-images/h5/mine-new-black-close-eye.png';
    },
    formatNumber(number) {
      if (!number) return 0;
      const num = Number(number);
      
      if (this.$appLanguage.isEnglish) {
        // English: convert to K (thousands) when >= 1000
        if (num >= 1000) {
          return parseFloat((num / 1000).toFixed(2));
        }
        return num;
      } else {
        // Chinese: convert to 万 (ten thousands) when >= 10000
        if (num >= 10000) {
          return parseFloat((num / 10000).toFixed(2));
        }
        return num;
      }
    },
  },
};
</script>