<template>
    <div class="aselect">
      <div class="selector" @click="toggle()">
        <div class="label">
          <span>{{ selected }}</span>
        </div>
        <div class="arrow" :class="{ expanded : visible }"></div>
        <div :class="{ hidden : !visible, visible }">
          <ul>
            <li :class="{ current : item === selected }" v-for="item in list" :key="item" @click="select(item.name)">{{ item.name }}</li>
          </ul>
        </div>
      </div>
    </div>
</template>

<script>
export default {
  props: {
    sortingList: Object,
    chosen: String
  },
  data () {
    return {
      visible: false,
      list: this.sortingList,
      selected: this.chosen
    }
  },
  methods: {
    toggle () {
      this.visible = !this.visible
    },
    select (option) {
      this.selected = option
      this.$emit('selected-option', option)
    }
  }
}
</script>

<style lang="scss">
    .aselect {
      min-width: 122px;
      margin: 0 0 16px;
      .selector {
        background: #FFFEFB;
        box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
        border-radius: 4px;
        position: relative;
        z-index: 1;
        .arrow {
          position: absolute;
          right: 12px;
          top: 40%;
          border: 1px solid #B4B4B4;
          border-top: 0;
          border-left: 0;
          width: 6px;
          height: 6px;
          transform: rotateZ(45deg) translateY(0px);
          transition-duration: 0.3s;
          transition-timing-function: cubic-bezier(.59,1.39,.37,1.01);
        }
        .expanded {
          transform: rotateZ(225deg) translateY(2px);
          top: 60%;
        }
        .label {
          display: block;
          padding: 10px 16px;
          font-size: 12px;
          color: #B4B4B4;
          cursor: pointer;
        }
      }
      ul {
        width: 100%;
        list-style-type: none;
        padding: 0;
        margin: 0;
        font-size: 12px;
        position: absolute;
        z-index: 1;
        background: #fff;
        box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
      }
      li {
        padding: 12px;
        color: #666;
        &:hover {
          color: white;
          background: #7BAE73;
          cursor: pointer;
        }
      }
      .current {
        background: #eaeaea;
      }
      .hidden {
        visibility: hidden;
      }
      .visible {
        visibility: visible;
      }
    }
</style>
