<script setup>
    import { computed } from 'vue';
    import { marked } from 'marked';
    import DOMPurify from 'dompurify';

const props = defineProps({
  message: Object
});

const renderedText = computed(() => {
    if (!props.message.text) return ""; // Handle image-only messages safely
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
                <div v-if="message.image" class="image-container">
                    <img :src="message.image" alt="Uploaded content" class="msg-image" />
                </div>
                <div v-if="message.text" class="markdown-text" v-html="renderedText"></div>                
                <div v-if="message.sources && message.sources.length" class="citations">
                <strong>Sources:</strong>
                <ul>
                    <li v-for="(source, index) in message.sources" :key="index">
                        <template v-if="typeof source === 'object'">
                            <a :href="source.url" target="_blank" class="source-link">
                            <span class="source-icon">
                                {{ source.type === 'quran' ? '📖' : '📜' }}
                            </span>
                            {{ source.label }}
                            <span class="external-icon">↗</span>
                            </a>
                        </template>
                        
                        <template v-else>
                            {{ source }}
                        </template>
                    </li>
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
    max-width: 85%;
    padding: 15px 20px;
    border-radius: 8px;
    position: relative;
    line-height: 1.5;
    box-shadow: 0 2px 5px rgba(0,0,0,0.05);
    }

    /* User Bubble Styles */
    .user .bubble {
    background-color: #1A472A;
    color: white;
    border-bottom-right-radius: 0;
    }

    /* AI Bubble Styles */
    .ai .bubble {
    background-color: #fff;
    border: 1px solid #C5A059; 
    border-left: 4px solid #C5A059; 
    color: #2C3E50;
    display: flex;
    gap: 15px;
    border-bottom-left-radius: 0;
    }

    .icon {
    font-size: 1.5rem;
    margin-top: -2px;
    }

    /* --- NEW IMAGE STYLES --- */
    .image-container {
        margin-bottom: 12px;
        overflow: hidden;
        border-radius: 8px;
    }

    .msg-image {
        display: block;
        max-width: 100%;       /* Ensure it fits in bubble */
        max-height: 300px;     /* Prevent it from being too tall */
        object-fit: cover;     /* Crop nicely if needed */
        border-radius: 6px;
        border: 1px solid rgba(0,0,0,0.1);
    }
    /* ------------------------ */

    .citations {
    margin-top: 15px;
    padding-top: 10px;
    border-top: 1px dashed #C5A059;
    font-size: 0.85rem;
    color: #555;
    }

    .citations ul {
    list-style-type: none; 
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

    /* Markdown Styles */
    .markdown-text :deep(p) { margin: 0 0 10px 0; }
    .markdown-text :deep(p):last-child { margin-bottom: 0; }
    .markdown-text :deep(strong) { color: inherit; font-weight: 700; } /* Inherit color so it looks good in Green bubble too */
    .markdown-text :deep(ul), .markdown-text :deep(ol) { margin: 5px 0 15px 20px; padding: 0; }
    .markdown-text :deep(li) { margin-bottom: 5px; }

    /* Code block fix for User vs AI bubble contrast */
    .markdown-text :deep(code) {
        background-color: rgba(0,0,0,0.1); /* Translucent so it works on green & white */
        padding: 2px 5px;
        border-radius: 4px;
        font-family: monospace;
        font-size: 0.9em;
        color: inherit; /* Inherit text color */
    }

    .source-link {
    display: inline-flex;
    align-items: center;
    text-decoration: none;
    color: #1A472A; 
    font-weight: 600;
    transition: all 0.2s;
    border-bottom: 1px dashed #C5A059; 
    }

    .source-link:hover {
    color: #C5A059;
    border-bottom-style: solid;
    }

    .source-icon { margin-right: 8px; font-size: 1.1em; }
    .external-icon { font-size: 0.8em; margin-left: 5px; opacity: 0.6; }
    .citations li { margin-bottom: 8px; }
</style>