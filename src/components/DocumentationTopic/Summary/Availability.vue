<!--
  This source file is part of the Swift.org open source project

  Copyright (c) 2021 Apple Inc. and the Swift project authors
  Licensed under Apache License v2.0 with Runtime Library Exception

  See https://swift.org/LICENSE.txt for license information
  See https://swift.org/CONTRIBUTORS.txt for Swift project authors
-->

<template>
  <Section class="availability" role="complementary" aria-label="Availability">
    <Title>Availability</Title>
    <List class="platform-list">
      <Item
        v-for="platform in platforms"
        :key="platform.name"
        :class="changesClassesFor(platform.name)"
        class="platform"
        :change="!!changeFor(platform.name)"
      >
        <AvailabilityRange
          :deprecatedAt="platform.deprecatedAt"
          :introducedAt="platform.introducedAt"
          :platformName="platform.name"
        />
        <Badge v-if="platform.deprecatedAt" variant="deprecated" />
        <Badge v-else-if="platform.beta" variant="beta" />
      </Item>
    </List>
  </Section>
</template>

<script>
import Badge from 'docc-render/components/Badge.vue';
import { getAPIChanges } from 'docc-render/mixins/apiChangesHelpers';
import { ChangeTypes } from 'docc-render/constants/Changes';
import AvailabilityRange from './AvailabilityRange.vue';
import List from './List.vue';
import ListItem from './ListItem.vue';
import Section from './Section.vue';
import Title from './Title.vue';

export default {
  name: 'Availability',
  mixins: [getAPIChanges],
  inject: ['identifier', 'store'],
  components: {
    Badge,
    AvailabilityRange,
    Item: ListItem,
    List,
    Section,
    Title,
  },
  props: {
    platforms: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      state: this.store.state,
    };
  },
  methods: {
    changeFor(platform) {
      const { identifier, state: { apiChanges } } = this;
      const { availability = {} } = (apiChanges || {})[identifier] || {};
      const changeData = availability[platform];

      if (!changeData) {
        return undefined;
      }

      if (changeData.deprecated) {
        return ChangeTypes.deprecated;
      }

      if (changeData.introduced) {
        if (!changeData.introduced.previous) {
          return ChangeTypes.added;
        }
      }

      return ChangeTypes.modified;
    },
  },
};
</script>

<style scoped lang="scss">
@import 'docc-render/styles/_core.scss';

.availability, .platform-list, .platform {
  box-sizing: inherit;
}

.platform {
  padding-right: 2rem;
  box-sizing: border-box;

  &::after {
    width: $change-total-icon-width;
    height: $change-total-icon-width;
    // Baseline align with first line of text
    margin-top: 6px;
  }

  &-badge {
    margin-left: rem(8px);
  }

  @include change-highlight-horizontal-spacing();
  padding-left: 0;

  margin-bottom: 0.25rem;
  &:last-child {
    margin-bottom: 0;
  }

  @include change-highlight-vertical-spacing();

  &-badge {
    margin-left: rem(8px);
  }

  &.changed {
    &::after {
      width: $change-total-icon-width;
      height: $change-total-icon-width;
      // Baseline align with first line of text
      margin-top: 6px;
    }

    @include change-highlight-horizontal-text-alignment();
  }
}
</style>
