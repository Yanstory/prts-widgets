<script setup lang="ts">
import { NAvatar, NBadge, NCard } from "naive-ui";

import { TORAPPU_ENDPOINT } from "@/utils/consts";
import { getImagePath } from "@/utils/utils";
defineProps<{
  title?: string;
  type?: string;
  icon?: string;
  iconId?: string;
  desc1?: string;
  desc2?: string;
  isTheme?: string;
  customBadgeText?: string;
}>();
</script>

<template>
  <NCard
    v-if="title || desc1 || desc2"
    :style="
      type === 'desc'
        ? { cursor: 'default' }
        : { cursor: 'pointer', borderColor: '#929292' }
    "
    :title="type === 'guide' ? '' : title"
    size="small"
    :header-style="{ height: '3em' }"
  >
    <template v-if="icon" #header-extra>
      <NAvatar
        v-if="type === 'simple'"
        color="#212121"
        object-fit="scale-down"
        :size="40"
        :src="getImagePath(`集成战略_选项_${icon}.png`)"
      />
      <NAvatar
        v-if="type === 'item'"
        color="#212121"
        object-fit="scale-down"
        :size="40"
        :src="getImagePath(`集成战略_道具_${icon}.png`)"
      />
      <NBadge v-if="type === 'collection'" :value="icon" color="#666666">
        <NAvatar
          color="#212121"
          object-fit="scale-down"
          :size="40"
          :src="
            getImagePath(
              isTheme ? `收藏品_${isTheme}_${icon}.png` : `收藏品_${icon}.png`,
            )
          "
        />
      </NBadge>
      <NBadge v-if="type === 'collection2'" :value="icon" color="#666666">
        <NAvatar
          color="#212121"
          object-fit="scale-down"
          :size="40"
          :src="`${TORAPPU_ENDPOINT}/assets/roguelike_topic_itempic/${iconId}.png`"
        />
      </NBadge>
      <NBadge
        v-if="type === 'custom' && customBadgeText"
        color="#666666"
        :offset="[-22.5, 0]"
      >
        <NAvatar
          color="#212121"
          object-fit="scale-down"
          :size="40"
          :src="getImagePath(`${icon}.png`)"
        />
        <template #value>
          <div class="w-max" v-html="customBadgeText"></div>
        </template>
      </NBadge>
      <NAvatar
        v-else-if="type === 'custom'"
        color="#212121"
        object-fit="scale-down"
        :size="40"
        :src="getImagePath(`${icon}.png`)"
      />
    </template>
    <div
      v-html="
        `${desc1}${type === 'guide' ? '<span class=\'mdi mdi-arrow-right float-right\'></span>' : ''}`
      "
    />
    <template v-if="desc2" #footer>
      <div class="text-xs" v-html="desc2" />
    </template>
  </NCard>
</template>
