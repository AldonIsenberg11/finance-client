<script lang="ts" async setup>
import { ArrowSmDownIcon, ArrowSmUpIcon } from '@heroicons/vue/solid'
import { inspect } from 'util';

const { data, pending, error, refresh } = await useFetch('http://localhost:6060/stockBreakdown')

let stock = data.value[0];

// try {
//   stock = JSON.parse(stock)
// } catch(e) {
//   stock = `Error parsing data: ${inspect(e)}`
// }

// const data = await $fetch('http://localhost:6060/stockBreakdown')
// const { data, pending, error, refresh } = await useFetch(
//   'http://localhost:6060/stockBreakdown',
//   {
//     headers: {
//       'Accept': 'application/json',
//     }
//   }
// )

const latestUpdate = new Date(stock.date).toDateString()

const stats = [
  // { name: 'Latest Update', stat: new Date(stock.date).toDateString(), previousStat: null, change: null, changeType: null },
  { name: '5 year score', stat: '58.16%', previousStat: '56.14%', change: '2.02%', changeType: 'increase' },
  { name: '3 year score', stat: '48.16%', previousStat: '36.14%', change: '3.02%', changeType: 'increase' },
  { name: '1 year score', stat: '24.57%', previousStat: '28.62%', change: '4.05%', changeType: 'decrease' },
]
</script>

<!-- This example requires Tailwind CSS v2.0+ -->
<template>
  <div>
    <h3 class="text-lg leading-6 font-medium text-gray-900">Default View [{{stock.ticker}}]</h3>
    <h3 class="text-lg leading-6 font-medium text-gray-900">Latest Update [{{latestUpdate}}]</h3>
    <dl class="mt-5 grid grid-cols-1 rounded-lg bg-white overflow-hidden shadow divide-y divide-gray-200 md:grid-cols-3 md:divide-y-0 md:divide-x">
      <div v-for="item in stats" :key="item.name" class="px-4 py-5 sm:p-6">
        <dt class="text-base font-normal text-gray-900">
          {{ item.name }}
        </dt>
        <dd class="mt-1 flex justify-between items-baseline md:block lg:flex">
          <div class="flex items-baseline text-2xl font-semibold text-indigo-600">
            {{ item.stat }}
            <span class="ml-2 text-sm font-medium text-gray-500"> from {{ item.previousStat }} </span>
          </div>

          <div :class="[item.changeType === 'increase' ? 'bg-green-100 text-green-800' : 'bg-red-100 text-red-800', 'inline-flex items-baseline px-2.5 py-0.5 rounded-full text-sm font-medium md:mt-2 lg:mt-0']">
            <ArrowSmUpIcon v-if="item.changeType === 'increase'" class="-ml-1 mr-0.5 flex-shrink-0 self-center h-5 w-5 text-green-500" aria-hidden="true" />
            <ArrowSmDownIcon v-else class="-ml-1 mr-0.5 flex-shrink-0 self-center h-5 w-5 text-red-500" aria-hidden="true" />
            <span class="sr-only"> {{ item.changeType === 'increase' ? 'Increased' : 'Decreased' }} by </span>
            {{ item.change }}
          </div>
        </dd>
      </div>
    </dl>
  </div>
</template>