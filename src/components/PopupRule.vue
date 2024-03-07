<template>
  <div class="popup" :class="getClassPopup">
    <div class="rule">
      <h1>Luật chơi</h1>
      <p>
        Đây là 1 trò chơi đơn giản. Khi đến lượt người chơi nào quay ấn roll đến số điểm bạn thân thấy muốn dừng thì
        dừng. Thỏa mãn số điểm ở dưới thì ấn hold là thắng. Nếu người chơi quay 2 ô xúc xắc ,1 trong 2 ô rơi vào 1 điểm
        toàn bộ số điểm mà sau khi ấn hold mà người chơi đó sẽ mất. Vậy nên nếu thấy số điểm vừa đủ hay ấn hold để
        chuyển lượt đừng quá tham lam nhé. Số điểm sẽ cộng dồn sau mỗi lần hold . Điểm để win bạn có thể tùy chỉnh trước
        khi ấn new game max là 100, khi ấn new game thì không thay đổi được điểm max, load lại trang để đổi điểm win và
        ấn new game nếu muốn chơi lâu hơn.
      </p>

      <button v-on:click="confirm" class="confirm">Đã Hiểu</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'popup-rule',
  props: {
    isOpenPopup: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    confirm() {
      this.$emit('handleConfirm');
    },
  },
  computed: {
    getClassPopup() {
      return {
        'open-popup': this.isOpenPopup,
      };
    },
  },
};
</script>

<style>
.popup {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0);
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
}
.open-popup {
  opacity: 1;
  visibility: visible;
}
.rule {
  width: 350px;
  height: 300px;
  background-color: #fff;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%) scale(1.2);
  position: absolute;
  transition: all 0.3s ease;
}
.open-popup .rule {
  transform: translate(-50%, -50%) scale(1);
}
.rule h1 {
  text-align: center;
  margin-top: 10px;
}
.rule .confirm {
  cursor: pointer;
  background-color: white;
  margin: 10px;
  transition: all 0.3s ease;
  padding: 10px;
  border: 1px solid black;
}
.rule .confirm:hover {
  color: white;
  background-color: black;
}
/* kết hợp css3 */
</style>