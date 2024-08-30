<script setup lang="ts">
import { ref, computed } from 'vue'
import { CaretSortIcon, CheckIcon } from '@radix-icons/vue'

import { cn } from '@/utils'
import { Button } from '@/components/ui/button'
import {
  Command,
  CommandEmpty,
  CommandGroup,
  CommandInput,
  CommandItem,
  CommandList,
} from '@/components/ui/command'
import {
  Popover,
  PopoverContent,
  PopoverTrigger,
} from '@/components/ui/popover'

const frameworks = [
  { value: '123', label: 'Next.js' },
  { value: '234', label: 'SvelteKit' },
  { value: '345', label: 'Nuxt' },
  { value: '456', label: 'Remix' },
  { value: '789', label: 'Astro' },
]

const open = ref(false)
const value = ref([])
const filterFunction = (list: string[], searchTerm: string): string[] => {
  return list.filter((option) => {
    return option.label.toLowerCase().includes(searchTerm.toLowerCase())
  })
}
const selectedValues = computed(() => {
  return value.value.map(v => v.value)
})
</script>

<template>
  <Popover v-model:open="open">
    <PopoverTrigger as-child>
      <Button
        variant="outline"
        role="combobox"
        :aria-expanded="open"
        class="w-[200px] justify-between"
      >
        {{ value.length
          ? value.length + ' selected'
          : "Select framework..." }}
        <CaretSortIcon class="ml-2 h-4 w-4 shrink-0 opacity-50" />
      </Button>
    </PopoverTrigger>
    <PopoverContent class="w-[200px] p-0">
      <Command multiple v-model="value" :filter-function="filterFunction">
        <CommandInput class="h-9" placeholder="Search framework..." />
        <CommandEmpty>No framework found.</CommandEmpty>
        <CommandList>
          <CommandGroup>
            <CommandItem
              v-for="framework in frameworks"
              :key="framework.value"
              :value="framework"

            >
              {{ framework.label }}
              <CheckIcon
                :class="cn(
                  'ml-auto h-4 w-4',
                  selectedValues.includes(framework.value) ? 'opacity-100' : 'opacity-0',
                )"
              />
            </CommandItem>
          </CommandGroup>
        </CommandList>
      </Command>
    </PopoverContent>
  </Popover>
</template>