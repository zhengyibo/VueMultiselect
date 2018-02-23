<template>
  <section>
    <div class="container" v-if="have">
      <header>
        <span>未选项 (单击选中)</span>
        <span>已选项</span>
      </header>
      <article class="multiselect">
        <div class="select-left">
          <button v-for="(item,index) in candidates"
            :key="index"
            :data-val="item.id"
            @click.stop.prevent="selectItem">
            {{ item.name }}
          </button>
        </div>
        <div class="select-right">
          <button v-for="(item,index) in selectedorganize"
            :key="index"
            :data-val="item.id"
            @click.stop.prevent="cancleClk">
            {{ item.name }}
          </button>
        </div>
      </article>
      <div class="select-middle">
        <span class="btn-switch" title="全选" @click.stop.prevent="selectItemAll">&gt;&gt;</span>
        <span class="btn-switch-b" title="清除" @click.left.stop.prevent="revocationItemAll">&lt;&lt;</span>
      </div>
    </div>
    <div v-else>
        {{ warning }}
    </div>
  </section>
</template>

<script>
export default {
    props: {
        candidates: {
            type: Array,
            require: true,
            default: []
        }
    },
    data () {
        return {
            have: false,
            selectedorganize: [],
            warning: '当前没有可选数据！'
        }
    },
    mounted () {
        this.candidates.sort(this.compare('name'))
        if (this.candidates.length > 0) {
            this.have = true
        }
    },
    methods: {
        selectItem (e) {
            // console.log()
            let aimVal = e.target.dataset.val
            let leftArr = this.candidates
            let aimIndex
            for (let i=0; i<leftArr.length; i++) {
               if (leftArr[i].id === parseInt(aimVal)) {
                   aimIndex = i
                   break
               }s
            }
            this.selectedorganize.push(leftArr[aimIndex])
            this.selectedorganize.sort(this.compare('name'))
            this.candidates.splice(aimIndex, 1)
        },
        cancleClk (e) {
            // console.log(e)
            let aimVal = e.target.dataset.val
            let rightArr = this.selectedorganize
            let aimIndex
            for (let i=0; i<rightArr.length; i++) {
               if (rightArr[i].id === parseInt(aimVal)) {
                   aimIndex = i
                   break
               }
            }
            this.candidates.push(rightArr[aimIndex])
            this.candidates.sort(this.compare('name'))
            this.selectedorganize.splice(aimIndex, 1)
        },
        selectItemAll () {
            this.candidates.forEach(function(element) {
                this.selectedorganize.push(element)   
            }, this)
            this.selectedorganize.sort(this.compare('name'))
            this.candidates.splice(0, this.candidates.length)
        },
        revocationItemAll () {
            this.selectedorganize.forEach(function(element) {
                this.candidates.push(element)   
            }, this)
            this.candidates.sort(this.compare('name'))
            this.selectedorganize.splice(0, this.selectedorganize.length)
        },
        compare (prop) {
            return function (obj1, obj2) {
                let val1 = obj1[prop]
                let val2 = obj2[prop]
                if (typeof val1 === 'string') {
                    val1 = val1.charAt(0).charCodeAt()
                }
                if (typeof val2 === 'string') {
                    val2 = val2.charAt(0).charCodeAt()
                }
                return val1 - val2
            }
        }
    },
    watch: {
        'selectedorganize' () {
            this.$emit('input', this.selectedorganize)
        }
    }
}
</script>


<style scoped>
.container {
  display: block;
  width: 100%;
  height: 100%;
  position: relative;
}
.container > header {
  display: flex;
  flex-direction: row;
  border-right: 1px solid rgba(200, 200, 200, 0.9);
  border-left: 1px solid rgba(200, 200, 200, 0.9);
  border-top: 1px solid rgba(200, 200, 200, 0.9);
  border-radius: 4px;
}
.container > header > span {
  flex: 1;
  text-align: center;
  color: 14px;
  height: 30px;
  line-height: 30px;}

.multiselect {
  display: flex;
  flex-direction: row;
  position: relative;
  width: 100%;
  height: 84%;
  overflow-y: auto;
  border-radius: 4px;
  border: 1px solid rgba(200, 200, 200, 0.9);
  align-items: top;
  background-color: white; }

.select-left {
  flex: 2;
  margin-right: 20%;
  height: auto;
  padding: .5rem 0 .5rem .5rem; }

.select-middle {
  display: block;
  width: 20%;
  height: 85%;
  position: absolute;
  left: 40%;
  top: 30px;
  border-left: 1px solid rgba(200, 200, 200, 0.9);
  border-right: 1px solid rgba(200, 200, 200, 0.9); }
  .select-middle .btn-switch, .select-middle .btn-switch-b {
    display: block;
    margin: 50% auto 0px;
    height: 30px;
    line-height: 30px;
    width: 30px;
    text-align: center;
    cursor: pointer;
    font-weight: bold;
    transform: scale(1.2); }
  .select-middle .btn-switch-b {
    margin: 0px auto 40%; }

.select-right {
  flex: 2;
  margin-left: 25px;
  margin-right: .5rem;
  padding: .5rem 0 .5rem 0; }
  .select-right > button {
    padding: 0px;
    background-color: rgba(200, 200, 200, 0.8); }

button {
  display: block;
  padding: 0 .5rem;
  margin-bottom: .2rem;
  border: 0px;
  width: 100%;
  cursor: pointer;
  line-height: 1.5;
  background-color: transparent;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  text-align: left; }

</style>


