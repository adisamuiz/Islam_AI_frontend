<script setup>
    import axios from "axios"
    import { ref, nextTick } from 'vue';
    import ChatHeader from './components/ChatHeader.vue';
    import MessageBubble from './components/MessageBubble.vue';
    import ChatInput from './components/ChatInput.vue';
    import SideMenu from './components/SideMenu.vue';

    const isSidebarOpen = ref(false);
    const chatWindow = ref(null);

    // State to hold the conversation
    const messages = ref([
    {
        id: 1,
        role: 'ai',
        text: 'Assalamu Alaykum. I am Al-Bayan. Ask me anything, and I will answer strictly from the Quran, Sunnah, and trusted Tafsir.',
        sources: [] 
    }
    ]);

    // Function to handle sending a message
    const handleNewMessage = async ({ text, file }) => {
        const imagePreviewUrl = file ? URL.createObjectURL(file) : null
        // 1. Add User Message
        messages.value.push({ 
            role: 'user', 
            text: text,
            image: imagePreviewUrl // Save image URL for display
        });

        // Scroll down so user sees their message
        await nextTick();
        scrollToLatestMessage();

        const formData = new FormData();
        formData.append("message", text); 
        if (file) {
            formData.append("image", file);
        }

        try {
            const response = await axios.post(/*"https://islam-ai-chatbot.onrender.com/api/chat"*/"http://localhost:3000/api/chat", formData)
            console.log('Server response:', response.data)
            messages.value.push({
                role: 'ai',
                text: response.data.reply,     
                sources: response.data.sources || []
            });
        } 
        catch (error) {
            console.error("Error fetching data:", error)
            messages.value.push({
                role: 'ai',
                text: "I encountered an error connecting to the server.",
                isError: true
            })
        }

        await nextTick();
        scrollToLatestMessage();
        };

        const scrollToLatestMessage = () => {
        if (!chatWindow.value) return;
            const messageElements = chatWindow.value.querySelectorAll('.message-wrapper');
            const lastMessage = messageElements[messageElements.length - 1];
            if (lastMessage) {
                lastMessage.scrollIntoView({ 
                behavior: 'smooth', 
                block: 'start' 
                });
            }
    };
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
