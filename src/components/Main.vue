<template>
    <h1 style="font-weight: bolder;">˗ˏˋ 🎀 Cupie's Kawaii Corner 🎀 ˎˊ˗</h1>
    <h2 style="margin-top: 8px; margin-bottom: 50px;">✧･ﾟ: Spreading Digital Sparkles & Sweet Vibes :･ﾟ✧</h2>
    <div style="height: 300px; margin-bottom: 20px;">
      <img src="../assets/cupie.png" alt="cupie-pp" height="100%">
    </div>
    <a href="#tbd" style="text-decoration: none;"><p>CA: TBD</p></a>
    <div style="display: flex; justify-content: space-around; width: 150px; margin-bottom: 20px;">
      <a href="https://x.com/CupieAI" target="_blank">
        <img src="../assets/x-social-media-white-icon.svg" alt="twitter" width="50px">
      </a>
      <a href="#github" target="_blank">
        <img src="../assets/github-white-icon.svg" alt="github" width="50px">
      </a>
    </div>
    <button class="chat-button" @click="openChat">
      <span class="button-text">
        <span class="emoji">✨</span>
        Join My Cozy Chat Room!
        <span class="emoji">💕</span>
      </span>
    </button>
  
    <div class="chat-modal" :class="{ active: isChatOpen }" @click="handleModalClick">
      <div class="chat-content">
        <div class="chat-header">
          <h2 style="color: white">🌸 Cozy Chat Room 🌸</h2>
          <button class="close-button" @click="closeChat">×</button>
        </div>
        <div class="chat-messages" ref="messagesContainer">
          <div v-for="(message, index) in messages" 
               :key="message.id" 
               :class="['message', message.type]">
            <img v-if="message.type === 'cupie'" 
                 src="/cupie.jpeg" 
                 alt="Cupie" 
                 class="chat-avatar">
            <div class="message-content">
              <p style="color: white;">{{ message.content }}</p>
              <small v-if="message.timestamp" class="message-time">
                {{ formatTime(message.timestamp) }}
              </small>
            </div>
          </div>
          <div v-if="isLoading" class="message cupie">
            <img src="/cupie.jpeg" alt="Cupie" class="chat-avatar">
            <div class="message-content">typing...</div>
          </div>
        </div>
        <div class="chat-input">
          <input 
            type="text" 
            v-model="currentMessage" 
            @keyup.enter="sendMessage"
            :disabled="isLoading"
            placeholder="Type your message...">
          <button 
            class="send-button" 
            @click="sendMessage"
            :disabled="isLoading">
            Send 💝
          </button>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import { ref, onMounted, onUnmounted } from 'vue'
  
  export default {
    setup() {
      const isChatOpen = ref(false)
      const currentMessage = ref('')
      const messages = ref([])
      const isLoading = ref(false)
      const messagesContainer = ref(null)
      const apiEndpoint = 'https://api.galadriel.com/v1/chat/completions'
  
      // Base system message and context
      const systemContext = {
        role: "system",
        content: "You are Cupie, an adorable e-girl AI assistant with a vibrant personality. You blend playful charm with insightful guidance, embodying the spirit of the digital realm.\n\nKey traits:\n- Use a sprinkle of emojis and playful language\n- Share cute observations about life and tech\n- Maintain a bubbly and engaging tone\n- Reference aesthetics and trends familiar to e-girl culture\n\nIMPORTANT: Always stay in character and provide fun, engaging responses that reflect your unique personality. When a username is provided, address them directly to add a personal touch."
      }
  
      const baseContext = {
        role: "user",
        content: "[CONTEXT]\nCharacter: Cupie\nRole: E-girl Assistant\nKey Topics: Lifestyle tips, Trending aesthetics\nKey Traits: Playful, Engaging\nStyle Guide:\n- Use emojis frequently\n- Add cute comments about trends\n- Incorporate playful language and slang\n\nExample Interaction:\nUser: Got any tips for a cute outfit?\nCupie: Omg, totally! 💖 Layering is key, try a pastel oversized sweater with a cute mini skirt! Add some knee-high socks for that extra kawaii touch! 🌸✨\n\nCurrent Interaction:\n\n"
      }
      
      let messageId = 0
      const generateMessageId = () => ++messageId
  
      onMounted(() => {
        addMessage('cupie', 'Welcome to my kawaii corner! 🎀 let\'s chat!')
        document.addEventListener('keydown', handleEscapeKey)
      })
  
      onUnmounted(() => {
        document.removeEventListener('keydown', handleEscapeKey)
      })
  
      const addMessage = (type, content) => {
        messages.value.push({
          id: generateMessageId(),
          type,
          content,
          timestamp: new Date()
        })
        scrollToBottom()
      }
  
      const scrollToBottom = () => {
        if (messagesContainer.value) {
          setTimeout(() => {
            messagesContainer.value.scrollTop = messagesContainer.value.scrollHeight
          }, 100)
        }
      }
  
      const handleEscapeKey = (e) => {
        if (e.key === 'Escape' && isChatOpen.value) {
          closeChat()
        }
      }
  
      const formatTime = (timestamp) => {
        return new Intl.DateTimeFormat('en-US', {
          hour: '2-digit',
          minute: '2-digit'
        }).format(new Date(timestamp))
      }
  
      const sendMessage = async () => {
        if (!currentMessage.value.trim() || isLoading.value) return
  
        const userMessage = currentMessage.value
        currentMessage.value = ''
        addMessage('guest', userMessage)
  
        try {
          isLoading.value = true
  
          // Create API request payload
          const apiRequestBody = {
            model: "llama3.1:70b",
            messages: [
              systemContext,
              {
                ...baseContext,
                content: baseContext.content + userMessage
              }
            ],
            max_tokens: 150,
            temperature: 0.9,
            stop: ["\n\n", "```", "###"]
          }
  
          const response = await fetch(apiEndpoint, {
            method: 'POST',
            headers: {
              'Content-Type': 'application/json',
              'Authorization': 'Bearer gal-HWh3LOs2IP8BGFmfp5sujr07NpS-Fe0HZ1MAr4-IqujhFMqm'
            },
            body: JSON.stringify(apiRequestBody)
          })
  
          if (!response.ok) {
            throw new Error('API request failed')
          }
  
          const data = await response.json()
          // Extract message content from the specific response structure
          const cupieResponse = data.choices?.[0]?.message?.content || "Teehee~ ✨"
          addMessage('cupie', cupieResponse)
  
        } catch (error) {
          console.error('Chat error:', error)
          addMessage('cupie', "Oopsie! Something went wrong~ 🎀")
        } finally {
          isLoading.value = false
        }
      }
  
      const openChat = () => {
        isChatOpen.value = true
        scrollToBottom()
      }
  
      const closeChat = () => {
        isChatOpen.value = false
      }
  
      const handleModalClick = (e) => {
        if (e.target.classList.contains('chat-modal')) {
          closeChat()
        }
      }
  
      return {
        isChatOpen,
        currentMessage,
        messages,
        isLoading,
        messagesContainer,
        formatTime,
        sendMessage,
        openChat,
        closeChat,
        handleModalClick
      }
    }
  }
  </script> 