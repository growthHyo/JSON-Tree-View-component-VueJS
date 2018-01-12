<template>
  <div class="container" id="app">  
    <div class="row">
      <div class="col-sm-4">
        <h2>JSON</h2>
        <pre><code>{{formatJSON(sampleData)}}</code></pre>
      </div>
      <div class="col-sm-1">
          </div>
      <div class="col-sm-7">
        <h2>Tree View Component</h2>
        <tree-view :data="sampleData"></tree-view>
      </div>
    </div>
  </div>
</template>

<script>
import TreeView from './components/TreeView'

export default {
  name: 'app',
  components: {
    TreeView
  },
  methods: {
    // return pretty-printed JSON
    formatJSON: theJSON => JSON.stringify(theJSON, true, 2)
  },
  data: function () {
    return {
      // Returns a JSON object with common
      // data structures, such as Arrays,
      // Objects and raw Values
      sampleData: {
        keyString: 'b',
        keyArray: [{ a: 'b' }, { c: 'd', e: 'f' }],
        keyObject: {
          keyString: 'b',
          keyNumber: 1234,
          keyArray: [
            1,
            'a',
            {
              keyString: 'd'
            }
          ]
        },
        keyNull: null,
        keyNumber: 3.1415
      }
    }
  }
}
</script>

<style lang="scss">
@import "../node_modules/bootstrap/scss/bootstrap.scss";
pre {
  background: #eee;
  border: 1px solid #ddd;
}
</style>