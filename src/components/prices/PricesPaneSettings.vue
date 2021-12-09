<template>
  <div>
    <div class="form-group mb8">
      <label class="checkbox-control" v-tippy="{ placement: 'left' }" title="ex: BTC-USD">
        <input type="checkbox" class="form-control" :checked="showPairs" @change="$store.commit(paneId + '/TOGGLE_PAIRS')" />
        <div></div>
        <span>Show pairs</span>
      </label>
    </div>

    <div class="form-group mb8">
      <label class="checkbox-control">
        <input type="checkbox" class="form-control" :checked="showVolume" @change="$store.commit(paneId + '/TOGGLE_VOLUME')" />
        <div></div>
        <span>Show volume</span>
      </label>
    </div>

    <div class="form-group mb8">
      <label class="checkbox-control">
        <input type="checkbox" class="form-control" :checked="showChange" @change="$store.commit(paneId + '/TOGGLE_CHANGE')" />
        <div></div>
        <span>Show change</span>
      </label>
    </div>

    <div class="form-group mb8">
      <label class="checkbox-control">
        <input type="checkbox" class="form-control" :checked="animateSort" @change="$store.commit(paneId + '/TOGGLE_SORT_ANIMATION')" />
        <div></div>
        <span>Order change animations are {{ animateSort ? 'enabled' : 'disabled' }}</span>
      </label>
    </div>

    <div class="form-group mb8">
      <label>Sort by</label>
      <div class="column">
        <dropdown
          class="-left"
          :selected="sortType"
          :options="['none', 'price', 'volume', 'change']"
          selectionClass="-outline form-control -arrow w-100"
          return-value
          @output="selectSortType($event)"
        ></dropdown>
        <label class="checkbox-control -auto" v-if="sortType">
          <input type="checkbox" class="form-control" :checked="sortOrder === 1" @change="$store.commit(paneId + '/TOGGLE_SORT_ORDER')" />
          <div on="ASC" off="DESC"></div>
        </label>
      </div>
    </div>

    <!--<small v-if="animateSort && disableAnimations" class="help-text mt8 mb16">
      Animations are disabled globaly !
      <a href="javascript:void(0);" @click="$store.commit('settings/TOGGLE_ANIMATIONS')">Enable animations</a>
    </small>-->
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator'
import Slider from '../framework/picker/Slider.vue'
import Thresholds from '../settings/Thresholds.vue'

@Component({
  components: { Thresholds, Slider },
  name: 'PricesSettings',
  props: {
    paneId: {
      type: String,
      required: true
    }
  }
})
export default class extends Vue {
  paneId: string

  get showPairs() {
    return this.$store.state[this.paneId].showPairs
  }

  get showVolume() {
    return this.$store.state[this.paneId].showVolume
  }

  get showChange() {
    return this.$store.state[this.paneId].showChange
  }

  get animateSort() {
    return this.$store.state[this.paneId].animateSort
  }

  get sortType() {
    return this.$store.state[this.paneId].sortType
  }

  get sortOrder() {
    return this.$store.state[this.paneId].sortOrder
  }

  selectSortType(option) {
    if (option === this.sortType) {
      this.$store.commit(this.paneId + '/TOGGLE_SORT_ASC')
    }

    this.$store.commit(this.paneId + '/SET_SORT_TYPE', option)
  }
}
</script>
