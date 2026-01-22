<script setup>
    import { ref } from 'vue';
    

    // Define events this component emits
    const emit = defineEmits(['send-message']);

    const inputText = ref('');

    const send = () => {
    if (inputText.value.trim()) {
        emit('send-message', inputText.value); // Send data up to App.vue
        inputText.value = ''; // Clear input
    }
    };
    // import axios from "axios"
    // import { ref } from "vue"
    
    // const inputMessage = ref("")
    // const responseMessage = ref(null)

    // async function submitData() {
        // try {
        //     const response = await axios.post("http://localhost:3000/api/chat", {
        //         message: inputMessage.value
        //     })

        //     responseMessage.value = response.data.reply
        //     console.log('Server response:', response.data)
        // } catch (error) {
        //     console.error("Error fetching data:", error)
        // }
    // }
</script>

<template>
    <div class="input-area">
        <input 
        v-model="inputText" 
        @keyup.enter="send"
        type="text" 
        placeholder="Ask a question about Islam..." 
        />
        <button @click="send" :disabled="!inputText.trim()">
            <span class="send-icon">➤</span>
        </button>
    </div>
    <!-- <div>
        <input type="text" v-model="inputMessage" placeholder="Type your message here">
        <button @click="submitData">Send to Server</button>
    </div>
    <div v-if="responseMessage">
        <h2>Response from Server:</h2>
        <p>{{ responseMessage }}</p>
    </div> -->
    
</template>

<style scoped>
    .input-area {
  padding: 20px;
  background-color: #fff;
  border-top: 1px solid #e0d8c3;
  display: flex;
  gap: 10px;
}

input {
  flex: 1;
  padding: 15px;
  border: 2px solid #e0d8c3;
  border-radius: 8px;
  font-family: 'Lato', sans-serif;
  font-size: 1rem;
  background-color: #FDFBF7;
  transition: border-color 0.3s;
}

input:focus {
  outline: none;
  border-color: #C5A059; /* Gold focus ring */
}

button {
  background-color: #1A472A;
  color: white;
  border: none;
  width: 50px;
  border-radius: 8px;
  cursor: pointer;
  font-size: 1.2rem;
  transition: background-color 0.3s;
}

button:hover:not(:disabled) {
  background-color: #143620;
}

button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}
</style>
