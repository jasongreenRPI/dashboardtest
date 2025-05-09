<script setup>
import { computed } from "vue";
import { useStore } from "vuex";
import { mdiMenu, mdiChevronLeft, mdiChevronRight } from "@mdi/js";
import AsideMenuList from "@/components/AsideMenuList.vue";
import NavBarItem from "@/components/NavBarItem.vue";
import Icon from "@/components/Icon.vue";

defineProps({
  menu: {
    type: Array,
    default: () => [],
  },
});

const store = useStore();

const asideStyle = computed(() => store.state.asideStyle);

const asideBrandStyle = computed(() => store.state.asideBrandStyle);

const asideMenuCloseLgStyle = computed(() => store.state.asideMenuCloseLgStyle);

const asideMenuLabelStyle = computed(() => store.state.asideMenuLabelStyle);

const isFullScreen = computed(() => store.state.isFullScreen);

const isAsideMobileExpanded = computed(() => store.state.isAsideMobileExpanded);

const isAsideLgActive = computed(() => store.state.isAsideLgActive);

const isAsideCollapsed = computed(() => store.state.isAsideCollapsed);

const asideLgClose = () => {
  store.dispatch("asideLgToggle", false);
};

const toggleCollapse = () => {
  store.dispatch("asideCollapseToggle");
};

const menuClick = (event, item) => {
  //
};
</script>

<template>
  <aside
    v-show="!isFullScreen"
    id="aside"
    class="fixed top-0 z-40 h-screen transition-all duration-300 ease-in-out lg:left-0 dark:border-r dark:border-gray-800 dark:bg-gray-900/70 lg:dark:bg-gray-900 xl:dark:bg-gray-900/70"
    :class="[
      asideStyle,
      isAsideMobileExpanded ? 'left-0' : '-left-60',
      isAsideLgActive ? 'block' : 'lg:hidden xl:block',
      isAsideCollapsed ? 'w-20' : 'w-60',
    ]"
  >
    <div
      class="flex flex-row w-full flex-1 h-14 items-center dark:bg-transparent"
      :class="[asideBrandStyle]"
    >
      <nav-bar-item
        type="hidden lg:flex xl:hidden"
        :active-color="asideMenuCloseLgStyle"
        active
        @click="asideLgClose"
      >
        <icon
          :path="mdiMenu"
          class="cursor-pointer"
          size="24"
        />
      </nav-bar-item>
      <div
        class="flex-1 px-3 flex items-center"
        :class="{
          'justify-center': isAsideCollapsed,
          'justify-between': !isAsideCollapsed,
        }"
      >
        <b v-if="!isAsideCollapsed">Hireflow</b>
        <nav-bar-item
          class="lg:flex"
          :active-color="asideMenuCloseLgStyle"
          active
          @click="toggleCollapse"
        >
          <icon
            :path="isAsideCollapsed ? mdiChevronRight : mdiChevronLeft"
            class="cursor-pointer"
            size="24"
          />
        </nav-bar-item>
      </div>
    </div>
    <div>
      <template v-for="(menuGroup, index) in menu">
        <p
          v-if="typeof menuGroup === 'string'"
          :key="`a-${index}`"
          class="p-3 text-xs uppercase"
          :class="[asideMenuLabelStyle, { 'opacity-0': isAsideCollapsed }]"
        >
          {{ menuGroup }}
        </p>
        <aside-menu-list
          v-else
          :key="`b-${index}`"
          :menu="menuGroup"
          :is-collapsed="isAsideCollapsed"
          @menu-click="menuClick"
        />
      </template>
    </div>
  </aside>
</template>
