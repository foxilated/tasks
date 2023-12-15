<script setup lang="ts">
import { get } from 'lodash'
import dayjs from 'dayjs'

import type { Props } from '~/common/types/props/table'

const props = defineProps<Props>()

const displayedFieldKeys = computed(() => {
  return Object.entries(props.fields).map(([_key, value]) => value.key)
})
</script>

<template>
  <div class="relative overflow-x-auto shadow-md sm:rounded-lg">
    <table
      w-full
      text="left sm slate-500 rtl:right dark:slate-400"
    >
      <thead
        text="dark:slate-400 xs slate-700"
        bg="dark:slate-700 slate-50"
      >
        <tr>
          <th
            v-for="field in fields"
            :key="field.key"
            class="px-6 py-3"
            v-text="field.label"
          />
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="item in items"
          :key="item.id"
          class="overflow-hidden rounded-full transition-all duration-300 ease-in-out"
          bg="even:white dark:even:slate-800 odd:slate-100 dark:odd:slate-900 odd:hover:dark:slate-700 even:hover:dark:slate-700"
        >
          <td
            v-for="key in displayedFieldKeys"
            :key="key"
            :class="key === 'path' ? 'px-6' : 'px-6 py-2'"
            class="min-w-max whitespace-nowrap"
          >
            <slot
              v-if="key === 'actions'"
              name="actions"
              :value="item.actions"
              :item="item"
            />
            <span
              v-else-if="key === 'creationDate'" text="slate-400"
              v-text="dayjs(item.creationDate).format('YYYY/MM/DD - hh:mm A')"
            />
            <slot
              v-else
              :name="`${key.replace('.', '-')}-field`"
              :value="item"
              :item="item"
            >
              <p
                v-if="get(item, key)"
                text="left slate-900 dark:slate-300 rtl:right"
                v-text="get(item, key)"
              />

              <span v-else>
                -
              </span>
            </slot>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
