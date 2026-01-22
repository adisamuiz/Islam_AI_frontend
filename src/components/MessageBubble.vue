<script setup>
    import { computed } from 'vue';
import { marked } from 'marked';
import DOMPurify from 'dompurify';

const props = defineProps({
  message: Object
});

// Create a computed property that automatically converts text to safe HTML
const renderedText = computed(() => {
  // 1. Convert Markdown to HTML
  const rawHtml = marked.parse(props.message.text);
  // 2. Sanitize the HTML (remove dangerous scripts)
  return DOMPurify.sanitize(rawHtml);
});
</script>

<template>
    <div :class="['message-wrapper', message.role]">
    
        <div class="bubble">
            <div v-if="message.role === 'ai'" class="icon">📖</div>
            
            <div class="content">
                <div class="markdown-text" v-html="renderedText"></div>
                
                <div v-if="message.sources && message.sources.length" class="citations">
                <strong>Sources:</strong>
                <ul>
                    <li v-for="source in message.sources" :key="source">{{ source }}</li>
                </ul>
                </div>
            </div>
        </div>

    </div>
</template>

<style scoped>
    .message-wrapper {
    display: flex;
    margin-bottom: 20px;
    }

    /* User Alignment */
    .message-wrapper.user {
    justify-content: flex-end;
    }

    /* AI Alignment */
    .message-wrapper.ai {
    justify-content: flex-start;
    }

    .bubble {
    max-width: 80%;
    padding: 15px 20px;
    border-radius: 8px;
    position: relative;
    line-height: 1.5;
    box-shadow: 0 2px 5px rgba(0,0,0,0.05);
    }

    /* User Bubble Styles - Simple & Clean */
    .user .bubble {
    background-color: #1A472A;
    color: white;
    border-bottom-right-radius: 0;
    }

    /* AI Bubble Styles - Scholarly/Parchment look */
    .ai .bubble {
    background-color: #fff;
    border: 1px solid #C5A059; /* Gold border */
    border-left: 4px solid #C5A059; /* Thick accent on left */
    color: #2C3E50;
    display: flex;
    gap: 15px;
    border-bottom-left-radius: 0;
    }

    .icon {
    font-size: 1.5rem;
    margin-top: -2px;
    }

    .citations {
    margin-top: 15px;
    padding-top: 10px;
    border-top: 1px dashed #C5A059;
    font-size: 0.85rem;
    color: #555;
    }

    .citations ul {
    list-style-type: none; /* Remove bullets */
    padding: 0;
    margin: 5px 0 0;
    }

    .citations li::before {
    content: "• ";
    color: #C5A059;
    }
    
    .markdown-text {
    line-height: 1.6;
    }

    /* We need 'deep' selectors because this HTML is injected dynamically */
    .markdown-text :deep(p) {
    margin: 0 0 10px 0;
    }

    .markdown-text :deep(p):last-child {
    margin-bottom: 0;
    }

    .markdown-text :deep(strong) {
    color: #1A472A; /* Make bold text Green for emphasis */
    font-weight: 700;
    }

    .markdown-text :deep(ul), .markdown-text :deep(ol) {
    margin: 5px 0 15px 20px;
    padding: 0;
    }

    .markdown-text :deep(li) {
    margin-bottom: 5px;
    }

    .markdown-text :deep(code) {
    background-color: #f3f0e6; /* Light parchment for code/terms */
    padding: 2px 5px;
    border-radius: 4px;
    font-family: monospace;
    font-size: 0.9em;
    color: #8b0000;
    }
</style>