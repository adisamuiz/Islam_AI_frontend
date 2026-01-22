<script setup>
    import axios from "axios"
    import { ref, nextTick } from 'vue';
    import ChatHeader from './components/ChatHeader.vue';
    import MessageBubble from './components/MessageBubble.vue';
    import ChatInput from './components/ChatInput.vue';
    import SideMenu from './components/SideMenu.vue';

    const isSidebarOpen = ref(false);

    // State to hold the conversation
    const messages = ref([
    {
        id: 1,
        role: 'ai',
        text: 'Assalamu Alaykum. I am Al-Bayan. Ask me anything, and I will answer strictly from the Quran, Sunnah, and trusted Tafsir.',
        sources: [] 
    }
    ]);

    const chatWindow = ref(null);

    // Function to handle sending a message
    const handleNewMessage = async (text) => {
    // 1. Add User Message
    messages.value.push({ role: 'user', text: text });

    // Scroll down so user sees their message
    await nextTick();
    scrollToBottom();

    try {
        const response = await axios.post("https://islam-ai-chatbot.onrender.com/api/chat", {
            message: text
        })

        console.log('Server response:', response.data)
        messages.value.push({
            role: 'ai',
            text: response.data.reply,     
            sources: response.data.sources || []
        });
    } 
    catch (error) {
        console.error("Error fetching data:", error)
    }

    await nextTick();
    scrollToBottom();
    };

    const scrollToBottom = () => {
    if (chatWindow.value) {
        chatWindow.value.scrollTop = chatWindow.value.scrollHeight;
    }
    };
    // import Header from "@/components/Header.vue"
    // import Main from "@/components/Main.vue"
    // import Footer from "@/components/Footer.vue"
</script>

<template>
    
    <div class="app-container">
        <SideMenu 
            :isOpen="isSidebarOpen" 
            @close="isSidebarOpen = false" 
        />
        <ChatHeader @toggle-menu="isSidebarOpen = !isSidebarOpen"/>
        <div class="chat-window" ref="chatWindow">
            <MessageBubble 
                v-for="(msg, index) in messages" 
                :key="index" 
                :message="msg" 
            />
        </div>

        <ChatInput @send-message="handleNewMessage" />
    </div>
    <!-- <Header/>
    <Main/>
    <Footer/> -->
</template>

<style>
    body {
        margin: 0;
        font-family: 'Lato', sans-serif;
        background-color: #FDFBF7; /* Parchment color */
    }

    .app-container {
        display: flex;
        flex-direction: column;
        height: 100vh;
        position: relative; /* Important for containing the layout */
        background-color: #FDFBF7;
    }

    .chat-window {
        flex: 1;
        overflow-y: auto;
        padding: 20px;
        background-image: radial-gradient(#e0d8c3 1px, transparent 1px);
        background-size: 20px 20px; /* Subtle dot pattern for texture */
    }
</style>
