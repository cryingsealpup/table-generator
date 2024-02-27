<!-- eslint-disable -->
<template>

    <div class="q-pa-md q-gutter-sm">
        <q-checkbox v-model="useFirstRowAsHeader" keep-color label="Использовать первую строку в качестве заголовков" color="teal" />
        <q-editor v-model="editor" min-height="5rem" />

        <q-card flat bordered>
            <q-card-section>
                <pre style="white-space: pre-line">{{ processedCode }}</pre>
            </q-card-section>
        </q-card>
    </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';

const editor = ref('Вставьте таблицу');
const useFirstRowAsHeader = ref(false);

const processedCode = computed(() => {
  const cleanedTags = editor.value.replace(/<\s*([a-z][a-z0-9]*)\s.*?>/gi, '<$1>');
  const removeHeading = cleanedTags.replace(/(<(div|span|p)>)+(<table>.*)$/g, '$3');
  const removeTrainling = removeHeading.replace(/^(.*<\/table>)(.*)$/g, '$1');
  const addLabel = useFirstRowAsHeader.value ? removeTrainling.replace(/(<th)(>)(.*?)(<\/th>)/g, '$1 data-label="$1"$2$3$4') : removeTrainling.replace(/(<tr>)((<td)(>)(.*?)(<\/td>))+(<\/tr>)/, '');
  const formatted = addLabel.replace(/(<table)(.*)/g, '$1 class="page-content__table"$2') || 'Здесь будет отображаться HTML-код';
  return formatted;
});

</script>
