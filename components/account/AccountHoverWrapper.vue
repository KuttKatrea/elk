<script setup lang="ts">
import type { mastodon } from 'masto'

defineOptions({
  inheritAttrs: false,
})

const props = defineProps<{
  account?: mastodon.v1.Account
  handle?: string
  disabled?: boolean
}>()

const account = computed(() => props.account || (props.handle ? useAccountByHandle(props.handle!) : undefined))
const userSettings = useUserSettings()
</script>

<template>
  <VMenu v-if="!disabled && account && !getPreferences(userSettings, 'hideAccountHoverCard')" placement="bottom-start" :delay="{ show: 500, hide: 100 }" v-bind="$attrs" :close-on-content-click="false">
    <slot />
    <template #popper>
      <AccountHoverCard v-if="account" :account="account" />
    </template>
  </VMenu>
  <slot v-else />
</template>
