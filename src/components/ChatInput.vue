<script setup>
    import { ref, watch } from 'vue';

    const emit = defineEmits(['send-message']);
    const userInput = ref("");
    const selectedFile = ref(null);
    const fileInputRef = ref(null); // Reference to the hidden input
    const previewUrl = ref(null)

    watch(selectedFile, (newFile) => {
        // 1. Cleanup old URL to avoid memory leaks
        if (previewUrl.value) {
            URL.revokeObjectURL(previewUrl.value);
            previewUrl.value = null;
        }
        
        // 2. Generate new URL if file exists
        if (newFile) {
            previewUrl.value = URL.createObjectURL(newFile);
        }
    });

    const triggerFileInput = () => {
        fileInputRef.value.click();
    };

    const handleFileSelect = (event) => {
        const file = event.target.files[0];
        if (file) {
            selectedFile.value = file;
        }
    };

    const clearFile = () => {
        selectedFile.value = null;
        if (fileInputRef.value) fileInputRef.value.value = ""; // Reset hidden input
    };

    const sendMessage = () => {
        if (!userInput.value.trim() && !selectedFile.value) return;

        emit('send-message', { 
            text: userInput.value, 
            file: selectedFile.value 
        });

        // Reset everything
        userInput.value = "";
        selectedFile.value = null; // The watcher handles cleaning up the URL
        if (fileInputRef.value) fileInputRef.value.value = "";
    };
</script>

<template>
    <div class="input-wrapper">
        <div v-if="previewUrl" class="input-image-preview">
                <div class="image-container">
                    <img :src="previewUrl" alt="Preview" class="preview-img" />
                    <button class="remove-btn" @click.stop="clearFile">×</button>
                </div>
        </div>
        <div class="input-bar">
            <input 
                type="file" 
                ref="fileInputRef" 
                @change="handleFileSelect" 
                accept="image/*" 
                style="display: none;" 
            />

            <button 
                class="icon-btn attach-btn" 
                @click="triggerFileInput" 
                :class="{ 'active': selectedFile }"
                title="Attach Image"
            >
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                    <path d="M21.44 11.05l-9.19 9.19a6 6 0 0 1-8.49-8.49l9.19-9.19a4 4 0 0 1 5.66 5.66l-9.2 9.19a2 2 0 0 1-2.83-2.83l8.49-8.48"></path>
                </svg>
            </button>

            <input 
                v-model="userInput" 
                @keyup.enter="sendMessage" 
                type="text" 
                placeholder="Ask a question about Islam..." 
            />
            
            <button class="send-btn" @click="sendMessage">
                ➤
            </button>
        </div>
    </div>
</template>

<style scoped>
.input-image-preview {
    padding: 10px 20px 0;
}

.image-container {
  position: relative;
  display: inline-block;
  border: 1px solid #e0d8c3;
  border-radius: 8px;
  overflow: hidden;
}

.image-container img {
  height: 60px;
  width: auto;
  display: block;
}

.preview-img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    border-radius: 6px;
    border: 1px solid #ddd;
}

.remove-btn {
  position: absolute;
  top: 0;
  right: 0;
  background: rgba(0,0,0,0.5);
  color: white;
  border: none;
  cursor: pointer;
  padding: 2px 6px;
}

.remove-btn:hover {
    background: #cc0000;
}
.input-bar {
  padding: 20px;
  display: flex;
  gap: 10px;
  align-items: center;
}
.input-wrapper {
    background-color: #fff;
    border-top: 1px solid #e0d8c3;
    display: flex;
    flex-direction: column;
}

input[type="text"] {
  flex: 1;
  padding: 15px;
  border: 2px solid #e0d8c3;
  border-radius: 8px;
  font-family: 'Lato', sans-serif;
  background-color: #FDFBF7;
  font-size: 1rem;
}

input:focus {
  outline: none;
  border-color: #C5A059;
}

.icon-btn {
    background: none;
    border: none;
    cursor: pointer;
    padding: 8px;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: background 0.2s, transform 0.1s;
    color: #555; /* Default gray color */
}

.icon-btn:hover {
    background-color: #f0f0f0;
}

.icon-btn:active {
    transform: scale(0.95);
}

/* SVG Sizing */
.icon-btn svg {
    width: 24px;
    height: 24px;
}

/* Attach Button Specifics */
.attach-btn.active {
    color: #1A472A; /* Green when file is selected */
    background-color: #e8f5e9;
}
.send-btn {
    background-color: #1A472A;
    color: white;
    border: none;
    height: 50px;
    border-radius: 8px;
    cursor: pointer;
    font-size: 1.2rem;
}

.send-btn { width: 50px; }

.send-btn:hover:not(:disabled) {
    background-color: #143620;
}

.send-btn:disabled { opacity: 0.5; }
</style>
