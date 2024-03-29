<!--
  This source file is part of the Swift.org open source project

  Copyright (c) 2021 Apple Inc. and the Swift project authors
  Licensed under Apache License v2.0 with Runtime Library Exception

  See https://swift.org/LICENSE.txt for license information
  See https://swift.org/CONTRIBUTORS.txt for Swift project authors
-->

<template>
  <span
    v-if="isCurrent"
    aria-current="page"
    aria-disabled="true"
    class="nav-menu-link current"
    role="link"
  >
    <slot />
  </span>
  <router-link
    v-else
    class="nav-menu-link"
    :to="url"
    tabindex="0"
  >
    <slot />
  </router-link>
</template>

<script>
import { buildUrl } from 'docc-render/utils/url-helper';

export default {
  name: 'NavMenuLink',
  computed: {
    isCurrent: ({ $route, url }) => ((typeof url === 'string') ? (
      // Use the `buildUrl` to compare urls, as we dont want to compare unnecessary query params
      // strip trailing slashes that may come from the URL
      url === buildUrl($route.path, $route.query).replace(/\/$/, '')
    ) : (
      url.name === $route.name
    )),
  },
  props: {
    url: {
      type: [Object, String],
      required: true,
    },
  },
};
</script>

<style lang='scss' scoped>
@import "docc-render/styles/_core.scss";

.nav-menu-link {
  display: inline-block;
  line-height: 22px;
  white-space: nowrap;

  @include nav-in-breakpoint {
    border-top: 1px solid;
    border-color: var(--color-nav-rule);
    display: flex;
    flex: 1 0 100%;
    height: 100%;
    align-items: center;

    @include nav-dark($nested: true) {
      border-color: var(--color-nav-dark-rule);
    }
  }
}
</style>
