<template>
    <div class="tree-view-item">
    	<div v-if="isObject(data)">
            <div class="tree-view-item-grouping" @click.stop="toggleOpen()">         	
                <span :class="{opened: isOpen(), 'hasProperties': !isOpen() && (data.children.length !== 1 || data.children.length === 1)}" v-if="!isRootObject(data)" class="tree-view-item-key tree-view-item-key-with-chevron">{{getKey(data)}}</span>
                <span class="tree-view-item-hint" v-show="!isOpen() && data.children.length === 1">{{data.children.length}} property</span>
                <span class="tree-view-item-hint" v-show="!isOpen() && data.children.length !== 1">{{data.children.length}} properties</span>
            </div>
            <tree-view-item v-show="isOpen()" v-for="(child, index) in data.children" :key="index" :data="child"></tree-view-item>
        </div>
    	<div v-if="isArray(data)">
            <div class="tree-view-item-grouping" @click.stop="toggleOpen()">
                <span :class="{opened: isOpen(), 'hasItems': !isOpen() && data.children.length !== 1}" v-if="!isRootObject(data)" class="tree-view-item-key tree-view-item-key-with-chevron">{{getKey(data)}}</span>
                <span class="tree-view-item-hint" v-show="!isOpen() && data.children.length === 1">{{data.children.length}} item</span>
                <span class="tree-view-item-hint"  v-show="!isOpen() && data.children.length !== 1">{{data.children.length}} items</span>
            </div>
            <tree-view-item v-show="isOpen()" v-for="(child, index) in data.children" :key="index" :data="child"></tree-view-item>
        </div>
    	<div v-if="isValue(data)">
            <span class="tree-view-item-key">{{getKey(data)}}</span>       
            <span class="tree-view-item-value" 
                :class="{
                        'isNumber': getValue(data)['isNumber'],
                        'isNull': getValue(data)['isNull'],
                        'isString': getValue(data)['isString']}">{{getValue(data)['value']}}</span>   
		</div>
    </div>
</template>

<script>
import _ from 'lodash'
import TreeViewItem from './TreeViewItem'

export default {
  name: 'tree-view-item',
  props: ['data', 'keyType'],
  data: () => {
    return {
      open: true
    }
  },
  components: {
    TreeViewItem
  },
  methods: {
    isOpen: function () {
      return this.isRootObject(this.data) || this.open
    },
    toggleOpen: function () {
      this.open = !this.open
    },
    isObject: value => value.type === 'object',
    isArray: value => value.type === 'array',
    isValue: value => value.type === 'value',
    getKey: value => {
      if (_.isInteger(value.key)) {
        return `${value.key}:`
      } else {
        return `"${value.key}":`
      }
    },
    getValue: value => {
      if (_.isNumber(value.value)) {
        return {'value': value.value, 'isNumber': true}
      }
      if (_.isNull(value.value)) {
        return {'value': 'null', 'isNull': true}
      }
      if (_.isString(value.value)) {
        return {'value': `"${value.value}"`, 'isString': true}
      }
      return {'value': `"${value.value}"`, 'isNext': true}
    },
    isRootObject: value => value.isRoot
  }
}
</script>

<style lang="scss">
.isString{
    color: red;
}
.isNumber{
    color: blue;
}
.isNull{
    color: grey;
}
.hasProperties{
    color: green;
}
.hasItems{
    color: orange;
}
.tree-view-item {
  font-family: monospace;
  font-size: 14px;
  margin-left: 18px;
}

.tree-view-item-grouping {
  cursor: pointer;
  position: relative;
}

.tree-view-item-key {
  font-weight: bold;
}

.tree-view-item-key-with-chevron {
  padding-left: 14px;
}

.tree-view-item-key-with-chevron.opened::before {
  top: 4px;
  transform: rotate(90deg);
  -webkit-transform: rotate(90deg);
}

.tree-view-item-key-with-chevron::before {
  color: #444;
  content: "\25b6";
  font-size: 10px;
  left: 1px;
  position: absolute;
  top: 3px;
  transition: -webkit-transform 0.1s ease;
  transition: transform 0.1s ease;
  transition: transform 0.1s ease, -webkit-transform 0.1s ease;
  -webkit-transition: -webkit-transform 0.1s ease;
}

.tree-view-item-hint {
  color: #ccc;
}
</style>

