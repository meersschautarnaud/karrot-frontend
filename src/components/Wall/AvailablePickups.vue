<template>
  <div>
    <q-card
      @click="showPickups = !showPickups"
      color="warning"
      class="generic-padding notice"
    >
      <i class="fa fa-exclamation-triangle on-left"/>
      {{ $tc('PICKUPLIST.AVAILABLE', pickups.length, { count: pickups.length }) }}
      <div
        class="card-arrow"
        :class="{ upsideDown: showPickups }"
      >
        <i class="fa fa-angle-down"/>
      </div>
    </q-card>
    <transition-group
      name="list"
      tag="div"
      v-if="showPickups"
    >
      <PickupItem
        v-for="pickup in pickups"
        :key="pickup.id"
        :pickup="pickup"
        @join="$emit('join', arguments[0])"
        @leave="$emit('leave', arguments[0])"
      >
        <strong v-if="pickup.store">
          <router-link :to="{ name: 'store', params: { storeId: pickup.store.id }}">
            {{ pickup.store.name }}
          </router-link>
        </strong>
        {{ $d(pickup.date, 'dateWithDayName') }}
      </PickupItem>
    </transition-group>
    <hr v-if="showPickups">
  </div>
</template>

<script>
import PickupItem from '@/components/Pickups/PickupItem'
import { QCard } from 'quasar'

export default {
  components: { PickupItem, QCard },
  props: {
    pickups: { required: true },
  },
  data () {
    return {
      showPickups: false,
    }
  },
}
</script>

<style scoped lang="stylus">
@import '~variables'
.list-leave-active
  transition: all .5s
.list-leave-to
  opacity: 0
  transform: translateX(-50px)
.q-card.notice
  cursor pointer
  transition: all .2s ease;
  color $primary !important
  .card-arrow
    float: right
    transition: all .3s ease;
  .upsideDown
    transform rotate(180deg)
.q-card.notice:hover
  box-shadow: 1px 2px 2px 1px rgba(0,0,0,0.4)
hr
  margin 1em 2em
  border solid lightgrey 1px;
</style>
