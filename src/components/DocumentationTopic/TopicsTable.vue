<!--
  This source file is part of the Swift.org open source project

  Copyright (c) 2021 Apple Inc. and the Swift project authors
  Licensed under Apache License v2.0 with Runtime Library Exception

  See https://swift.org/LICENSE.txt for license information
  See https://swift.org/CONTRIBUTORS.txt for Swift project authors
-->

<template>
  <ContentTable :anchor="anchor" :title="title">
    <ContentTableSection
      v-for="section in sectionsWithTopics"
      :key="section.title"
      :title="section.title"
    >
      <template v-if="wrapTitle" slot="title">
        <WordBreak tag="h3" class="title">
          {{ section.title }}
        </WordBreak>
      </template>
      <template v-if="section.abstract" slot="abstract">
        <ContentNode :content="section.abstract" />
      </template>
      <template v-if="section.discussion" slot="discussion">
        <ContentNode :content="section.discussion.content" />
      </template>
      <TopicsLinkBlock
        v-for="topic in section.topics"
        class="topic"
        :key="topic.identifier"
        :topic="topic"
        :isSymbolDeprecated="isSymbolDeprecated"
        :isSymbolBeta="isSymbolBeta"
      />
    </ContentTableSection>
  </ContentTable>
</template>

<script>
import ContentNode from 'docc-render/components/DocumentationTopic/ContentNode.vue';
import WordBreak from 'docc-render/components/WordBreak.vue';
import ContentTable from './ContentTable.vue';
import ContentTableSection from './ContentTableSection.vue';
import TopicsLinkBlock from './TopicsLinkBlock.vue';

export default {
  name: 'TopicsTable',
  inject: {
    references: {
      default() {
        return {};
      },
    },
  },
  components: {
    WordBreak,
    ContentTable,
    TopicsLinkBlock,
    ContentNode,
    ContentTableSection,
  },
  props: {
    isSymbolDeprecated: Boolean,
    isSymbolBeta: Boolean,
    sections: {
      type: Array,
      required: true,
    },
    title: {
      type: String,
      required: false,
      default() {
        return 'Topics';
      },
    },
    anchor: {
      type: String,
      required: false,
      default() {
        return 'topics';
      },
    },
    wrapTitle: {
      type: Boolean,
      default: false,
    },
  },
  computed: {
    sectionsWithTopics() {
      return this.sections.map(section => ({
        ...section,
        topics: section.identifiers.reduce(
          (list, id) => (this.references[id] ? list.concat(this.references[id]) : list),
          [],
        ),
      }));
    },
  },
};
</script>

<style scoped lang="scss">
@import 'docc-render/styles/_core.scss';

.topic:not(:last-child),
.section-content > .content {
  margin-bottom: $section-spacing-single-side / 2;
}
</style>
