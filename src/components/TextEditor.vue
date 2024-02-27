<!-- eslint-disable -->
<template>
    <div class="q-pa-md q-gutter-sm">
        <q-editor v-model="editor" min-height="5rem" />

        <q-card flat bordered>
            <q-card-section>
                <pre style="white-space: pre-line">{{ editor }}</pre>
            </q-card-section>
        </q-card>

        <q-card flat bordered>
            <q-card-section v-html="processedCode" />
        </q-card>
    </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const editor = ref('What you see is <b>what</b> you get.');
const processedCode = computed(() => editor.value.replace(/<table(.*)>/gm, 'class=page-content'));
</script>
