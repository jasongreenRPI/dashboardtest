<script setup>
import { mdiCog } from "@mdi/js";
import { computed } from "vue";
import { useStore } from "vuex";
import CardComponent from "@/components/CardComponent.vue";
import Icon from "@/components/Icon.vue";
import Level from "@/components/Level.vue";
import TrendPill from "@/components/TrendPill.vue";
import JbButton from "@/components/JbButton.vue";

defineProps({
  number: {
    type: Number,
    default: 0,
  },
  icon: {
    type: String,
    default: null,
  },
  prefix: {
    type: String,
    default: null,
  },
  suffix: {
    type: String,
    default: null,
  },
  label: {
    type: String,
    default: null,
  },
  color: {
    type: String,
    default: null,
  },
  trend: {
    type: String,
    default: null,
  },
  trendType: {
    type: String,
    default: null,
  },
  trendIcon: {
    type: String,
    default: null,
  },
});

const emit = defineEmits(["config-click"]);

const store = useStore();

const darkMode = computed(() => store.state.darkMode);

const handleConfigClick = () => {
  emit("config-click");
};
</script>

<template>
  <card-component>
    <level v-if="trend" class="mb-3" mobile>
      <trend-pill
        :trend="trend"
        :trend-type="trendType"
        :icon="trendIcon"
        small
      />
      <jb-button
        :icon="mdiCog"
        icon-w="w-4"
        icon-h="h-4"
        :color="darkMode ? 'white' : 'light'"
        :outline="darkMode"
        small
        @click="handleConfigClick"
      />
    </level>
    <level mobile>
      <div>
        <h3 class="text-lg leading-tight text-gray-500 dark:text-gray-400">
          {{ label }}
        </h3>
        <h1 class="text-3xl leading-tight font-semibold">
          {{ prefix }}{{ number }}{{ suffix }}
        </h1>
      </div>
      <icon v-if="icon" :path="icon" size="48" w="" h="h-16" :class="color" />
    </level>
  </card-component>
</template>
