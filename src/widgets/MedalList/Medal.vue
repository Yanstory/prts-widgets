<script lang="ts">
import { defineComponent, ref, type PropType } from "vue";

import { NCard, NConfigProvider, NImage, NTag, NTooltip } from "naive-ui";

import type { Medal } from "./types";

export default defineComponent({
  components: {
    NConfigProvider,
    NCard,
    NImage,
    NTag,
    NTooltip,
  },
  props: {
    medalData: {
      type: Object as PropType<Medal>,
      required: true,
    },
  },
  setup() {
    const rarityGradient: Record<number, string> = {
      1: "from-[#75655d] to-[#decaa2]",
      2: "from-[#83898b] to-[#7bb7b7]",
      3: "from-[#a56e37] to-[#f5c391]",
    };

    const rarityStarColor: Record<number, string> = {
      1: "color-white",
      2: "color-[#d7ffff]",
      3: "color-[#ffeebe]",
    };

    const rarityImgStyleSet: Record<number, string[]> = {
      1: ["p-8", "80"],
      2: ["p-6", "100"],
      3: ["p-4", "130"],
    };

    return {
      isDecrypt: ref(false),
      showTrimed: ref(false),
      rarityGradient,
      rarityStarColor,
      rarityImgStyleSet,
    };
  },
});
</script>

<template>
  <NConfigProvider
    preflight-style-disabled
    :theme-overrides="{
      Card: {
        paddingMedium: '0',
        borderColor: '#adadad',
      },
      Tag: {
        colorCheckable: '#565656',
        textColorCheckable: 'white',
        colorPressedCheckable: '#2E33387A',
        textColorPressedCheckable: 'white',
        colorChecked: '#1976d2',
        colorCheckedHover: '#52A5F7',
        colorCheckedPressed: '#707070FF',
      },
      Image: {
        toolbarColor: '#000000B3',
        toolbarBorderRadius: '5px',
      },
    }"
  >
    <NCard hoverable>
      <div class="flex <lg:flex-col">
        <div
          :class="[
            'bg-[#2f2f2f]/86 flex grid-items-center w-190px justify-center min-h-50',
            {
              'bg-gradient-to-b from-[#485a5c] to-[#1d0942]':
                showTrimed && medalData.isTrim,
            },
            '<lg:w-full <lg:py-2',
          ]"
        >
          <NImage
            :width="rarityImgStyleSet[medalData.rarity][1]"
            :src="`https://torappu.prts.wiki/assets/medal_icon/${showTrimed && medalData.isTrim ? medalData.trimId : medalData.id}.png`"
            show-toolbar-tooltip
            :previewed-img-props="{
              style: {
                padding: '20px',
                background: '#2f2f2fdb',
                borderRadius: '5px',
                justifyContent: 'center',
              },
            }"
          />
        </div>
        <div class="w-full flex flex-col divide-y!">
          <div
            :class="[
              'flex',
              'flex-row',
              'justify-between',
              'p-2.5! m-0! bg-gradient-to-r',
              'text-shadow-lg',
              rarityGradient[medalData.rarity],
            ]"
          >
            <h3 class="p-0 color-white mt-0! <lg:pt-1!">
              {{ medalData.name }}
            </h3>
            <div>
              <span
                v-for="i of medalData.rarity"
                :key="i"
                :class="[
                  'text-xl mdi mdi-star mdi-rotate-45',
                  rarityStarColor[medalData.rarity],
                ]"
              />
            </div>
          </div>
          <div
            class="flex grow items-center border-0 border-[#e5e7eb] border-solid align-middle p-2.5!"
          >
            <span
              class="whitespace-pre-wrap font-italic"
              v-html="medalData.desc"
            />
          </div>
          <div
            class="border-0 border-[#e5e7eb] border-solid align-middle p-2.5!"
          >
            <div v-if="!medalData.decrypt">
              <NTag
                :color="{
                  color: '#565656',
                  textColor: 'white',
                  borderColor: '#565656',
                }"
                >获得方式</NTag
              ><span class="pl-1" v-html="medalData.method"></span>
            </div>
            <div v-else>
              <NTooltip
                trigger="hover"
                :style="{ background: isDecrypt ? '#1976d2' : '#262626FF' }"
                :arrow-style="{
                  background: isDecrypt ? '#1976d2' : '#262626FF',
                }"
              >
                <template #trigger>
                  <NTag v-model:checked="isDecrypt" checkable>{{
                    isDecrypt ? "破译结果" : "获得方式"
                  }}</NTag>
                </template>
                点击{{ isDecrypt ? "加密" : "破译" }}蚀刻章获得方式
              </NTooltip>
              <span class="pl-1">{{
                isDecrypt ? medalData.decrypt : medalData.method
              }}</span>
            </div>
            <div v-if="medalData.isTrim" class="mt-0.5">
              <NTooltip
                trigger="hover"
                :style="{ background: '#262626FF' }"
                :arrow-style="{ background: '#262626FF' }"
              >
                <template #trigger>
                  <NTag
                    v-model:checked="showTrimed"
                    checkable
                    class="border-[#565656] from-[#b2ebf2] to-[#d1c4e9] bg-gradient-to-b font-bold color-[#2f2f2f]!"
                    >镀层方式</NTag
                  >
                </template>
                点击切换显示{{ showTrimed ? "未镀层" : "镀层" }}章
              </NTooltip>
              <span class="pl-1">{{ medalData.trimMethod }}</span>
            </div>
          </div>
        </div>
      </div>
    </NCard>
  </NConfigProvider>
</template>
