<script setup lang="ts">
import type { ViewType } from "~~/composables/use-preferences"
import type { ItemSummary } from "~~/lib/api/types/data-contracts"
import MdiDotsVertical from "~icons/mdi/dots-vertical"
import MdiCardTextOutline from "~icons/mdi/card-text-outline"
import MdiTable from "~icons/mdi/table"

type Props = {
  view?: ViewType
  items: ItemSummary[]
  more?: boolean
}

const preferences = useViewPreferences()

const props = defineProps<Props>()
const viewSet = computed(() => {
  return !!props.view
})

const itemView = computed(() => {
  return props.view ?? preferences.value.itemDisplayView
})

function setViewPreference(view: ViewType) {
  preferences.value.itemDisplayView = view
}
</script>

<template>
  <section>
    <BaseSectionHeader class="mb-2 flex justify-between items-center">
      {{ $t("headers.items") }}
      <template #description>
        <div v-if="!viewSet" class="dropdown dropdown-hover dropdown-left">
          <label tabindex="0" class="btn btn-ghost m-1">
            <MdiDotsVertical class="h-7 w-7" />
          </label>
          <ul
            tabindex="0"
            class="dropdown-content menu p-2 z-[1] shadow bg-base-100 rounded-box w-60"
          >
            <li class="menu-title">{{ $t("view.view") }}</li>
            <li>
              <button @click="setViewPreference('card')">
                <MdiCardTextOutline class="h-5 w-5" />
                {{ $t("view.card") }}
              </button>
            </li>
            <li>
              <button @click="setViewPreference('table')">
                <MdiTable class="h-5 w-5" />
                {{ $t("view.table") }}
              </button>
            </li>
            <li class="divide-y-2"></li>
            <li>
              <label class="label cursor-pointer">
                <span class="label-text ml-6">Display more</span>
                <input
                  v-model="preferences.itemDisplayMore"
                  type="checkbox"
                  class="toggle toggle-primary toggle-xs"
                />
              </label>
            </li>
          </ul>
        </div>
      </template>
    </BaseSectionHeader>

    <template v-if="itemView === 'table'">
      <ItemViewTable :more="preferences.itemDisplayMore" :items="items" />
    </template>
    <template v-else>
      <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-4">
        <ItemCard v-for="item in items" :key="item.id" :item="item" />
        <div class="first:block hidden text-lg">{{ $t("items.noItems") }}</div>
      </div>
    </template>
  </section>
</template>

<style scoped></style>
