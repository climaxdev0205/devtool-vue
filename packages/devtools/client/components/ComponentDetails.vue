<script setup lang="ts">
import type { Component } from 'nuxt/schema'
import { pascalCase } from 'scule'

const props = defineProps<{
  component: Component
}>()

// @ts-expect-error types
const name = computed(() => props.component.pascalName || pascalCase(props.component.name || props.component.__name || props.component.kebabName || ''))
// @ts-expect-error types
const filePath = computed(() => props.component.filePath || props.component.file || props.component.__file || '')
const copy = useCopy()
</script>

<template>
  <div flex="~ col gap1" items-start of-hidden>
    <div flex="~ gap2">
      <ComponentName :component="component" />
      <NIconButton title="Copy name" flex-none icon="carbon-copy" @click="copy(`<${name}></${name}>`)" />
      <Badge
        v-if="component.global"
        bg-green-400:10 text-green-400
        title="Registered at runtime as a global component"
        v-text="'runtime'"
      />
    </div>
    <div>
      <FilepathItem
        v-if="filePath"
        :filepath="filePath"
        w-full text-sm op25 group-hover:op75
      />
    </div>
    <slot />
  </div>
</template>
