<script setup>
import { ref, computed } from "vue";
import { useStore } from "vuex";
import { mdiMinus, mdiPlus } from "@mdi/js";
import Icon from "@/components/Icon.vue";
import AsideMenuList from "@/components/AsideMenuList.vue";

const props = defineProps({
  item: {
    type: Object,
    required: true,
  },
  isSubmenuList: Boolean,
  isCollapsed: Boolean,
});

const emit = defineEmits(["menu-click"]);

const store = useStore();

const asideMenuItemStyle = computed(() => store.state.asideMenuItemStyle);

const asideMenuItemActiveStyle = computed(
  () => store.state.asideMenuItemActiveStyle
);

const asideMenuItemInactiveStyle = computed(
  () => store.state.asideMenuItemInactiveStyle
);

const asideSubmenuListStyle = computed(() => store.state.asideSubmenuListStyle);

const isDropdownActive = ref(false);

const componentIs = computed(() => (props.item.to ? "router-link" : "a"));

const hasDropdown = computed(() => !!props.item.menu);

const dropdownIcon = computed(() =>
  isDropdownActive.value ? mdiMinus : mdiPlus
);

const itemTo = computed(() => props.item.to || null);

const itemHref = computed(() => props.item.href || null);

const itemTarget = computed(() =>
  componentIs.value === "a" && props.item.target ? props.item.target : null
);

const menuClick = (event) => {
  emit("menu-click", event, props.item);

  if (hasDropdown.value) {
    isDropdownActive.value = !isDropdownActive.value;
  }
};
</script>

<template>
  <li>
    <component
      :is="componentIs"
      v-slot="vSlot"
      :to="itemTo"
      :href="itemHref"
      :target="itemTarget"
      class="flex cursor-pointer dark:hover:bg-gray-700/50"
      :class="[asideMenuItemStyle, isSubmenuList ? 'p-3 text-sm' : 'py-2']"
      @click="menuClick"
    >
      <icon
        v-if="item.icon"
        :path="item.icon"
        class="flex-none"
        :class="[
          vSlot && vSlot.isExactActive
            ? asideMenuItemActiveStyle
            : asideMenuItemInactiveStyle,
        ]"
        :w="isCollapsed ? 'w-20' : 'w-12'"
      />
      <span
        v-if="!isCollapsed"
        class="grow"
        :class="[
          vSlot && vSlot.isExactActive
            ? asideMenuItemActiveStyle
            : asideMenuItemInactiveStyle,
        ]"
      >{{ item.label }}</span>
      <icon
        v-if="hasDropdown && !isCollapsed"
        :path="dropdownIcon"
        class="flex-none"
        :class="[
          vSlot && vSlot.isExactActive
            ? asideMenuItemActiveStyle
            : asideMenuItemInactiveStyle,
        ]"
        w="w-12"
      />
    </component>
    <aside-menu-list
      v-if="hasDropdown && !isCollapsed"
      :menu="item.menu"
      :class="[
        asideSubmenuListStyle,
        isDropdownActive ? 'block dark:bg-gray-800/50' : 'hidden',
      ]"
      is-submenu-list
      :is-collapsed="isCollapsed"
    />
  </li>
</template>
