<template>
  <th :class="cls" @click="triggerSort()" :rowspan="rowspan" :colspan="colspan">
    <div v-tooltip="tooltipParam.enable" :placement="tooltipParam.placement" :content="tooltipParam.content || title">
      <span>{{title}}</span>
      <span class="h-table-sort-handler" v-if="sort">
        <span class="h-table-sort-asc" v-if="sortStatus.type == 'asc' && sortStatus.prop == sortUseProp" :class="{'h-table-sort-selected sort-selected': sortStatus.type == 'asc' && sortStatus.prop == sortUseProp}"><i class="h-icon-top"></i></span>
        <span class="h-table-sort-desc" v-else :class="{'h-table-sort-selected sort-selected': sortStatus.type == 'desc' && sortStatus.prop == sortUseProp}"><i class="h-icon-down"></i></span>
      </span>
    </div>
  </th>
</template>

<script>
import utils from 'heyui/src/utils/utils';
export default {
  name: 'hTableTh',
  props: {
    rowspan: Number,
    colspan: Number,
    title: String,
    width: Number,
    className: String,
    fixed: String,
    label: String,
    prop: String,
    dict: String,
    align: { type: String, default: 'center' },
    format: Function,
    render: Function,
    unit: String,
    tooltip: {
      type: [Boolean, Object],
      default: false
    },
    sortProp: String,
    sort: {
      type: [Boolean, String],
      default: false
    },
    sortStatus: {
      type: Object,
      default: () => ({ type: null, prop: null })
    },
    placement: String,
    content: String
  },
  data() {
    return {
      // sortStatus: {type: null, prop: null}
    };
  },
  methods: {
    triggerSort() {
      if (!this.sort) return false;
      let sortStatus = utils.copy(this.sortStatus);
      if (this.sortStatus.type && this.sortStatus.prop == this.sortUseProp) {
        sortStatus.type = this.sortStatus.type == 'asc' ? 'desc' : 'asc';
      } else {
        sortStatus.type = 'desc';
        sortStatus.prop = this.sortUseProp;
      }
      let parent = this.$parent;
      if (parent.$options._componentTag == 'Table' || parent.$options._componentTag == 'h-table') {
        parent.triggerSort(sortStatus, this.sort);
      }
    }
  },
  computed: {
    tooltipParam() {
      if (this.tooltip === true) {
        return {
          enable: true,
          content: this.content,
          placement: this.placement
        };
      } else if (utils.isObject(this.tooltip)) {
        return {
          enable: true,
          content: this.tooltip.content,
          placement: this.tooltip.placement
        };
      }
      return {
        enable: false
      };
    },
    cls() {
      return {
        [`text-${this.align}`]: !!this.align,
        [this.className]: !!this.className,
        pointer: this.sort
      };
    },
    sortUseProp() {
      return this.sortProp || this.prop;
    }
  }
};
</script>
