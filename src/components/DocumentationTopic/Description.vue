<!--
  This source file is part of the Swift.org open source project

  Copyright (c) 2021 Apple Inc. and the Swift project authors
  Licensed under Apache License v2.0 with Runtime Library Exception

  See https://swift.org/LICENSE.txt for license information
  See https://swift.org/CONTRIBUTORS.txt for Swift project authors
-->

<template>
  <div class="description">
    <p v-if="!$slots.default && !hasOverview" class="nodocumentation">
      No overview available.
    </p>
    <slot />
  </div>
</template>

<script>
export default {
  name: 'Description',
  props: {
    hasOverview: {
      type: Boolean,
      default: () => false,
    },
  },
};
</script>

<style scoped lang="scss">
@import 'docc-render/styles/_core.scss';

.description:not(:empty) {
  margin-bottom: $contenttable-spacing-single-side;
}

.nodocumentation {
  @include font-styles(body-large);
  color: var(--colors-secondary-label, var(--color-secondary-label));
  margin-bottom: 0;
}

/deep/ .content + * {
  margin-top: $stacked-margin-large;
}
</style>
