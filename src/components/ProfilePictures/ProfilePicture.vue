<template>
  <div class="wrapper">
    <router-link
      v-if="isLink && user"
      :to="{name:'user', params: {userId: user.id}}"
    >
      <RandomArt
        :text="user.displayName"
        :seed="user.id"
        style="display: block; overflow: hidden"
        :style="{ width: size + 'px' , height: size + 'px' }"
      />
      <q-tooltip>
        {{ user.displayName }}
      </q-tooltip>
    </router-link>
    <div v-if="!isLink && user">
      <RandomArt
        :text="user.displayName"
        :seed="user.id"
        style="display: block; overflow: hidden"
        :style="{ width: size + 'px' , height: size + 'px' }"
      />
    </div>
    <span v-if="!user">
      <span>?</span>
      <q-tooltip>
        <span>
          {{ $t('PROFILE.INACCESSIBLE_OR_DELETED') }}
        </span>
      </q-tooltip>
    </span>
  </div>
</template>

<script>
import { QTooltip } from 'quasar'
import RandomArt from '@/components/General/RandomArt'

export default {
  props: {
    user: { required: true },
    size: { default: 20 },
    isLink: { default: true },
  },
  components: {
    QTooltip, RandomArt,
  },
}
</script>

<style scoped lang="stylus">
  .wrapper
    display inline-block
</style>
