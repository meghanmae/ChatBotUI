<template>
  <v-container>
    <MessageCard :message="message" v-for="(message, i) in messages" :key="i" />

    <MessageCard
      id="animated-block"
      :message="animatedMessage"
      v-if="animatedMessage.message || thinking"
    />
  </v-container>

  <v-sheet color="transparent" height="170px" />
  <v-card class="message-box mt-10">
    <v-text-field
      hide-details
      @keyup.enter="send"
      v-model="question"
      @click:append-inner="send"
      append-inner-icon="mdi-send"
      placeholder="Ask a question..."
    />
  </v-card>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import MessageCard from '@/components/MessageCard.vue'
import { Message } from '@/scripts/Message'

const testText =
  'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vestibulum arcu massa, malesuada vel dignissim ac, euismod a mauris. Sed ultrices ante sit amet urna luctus, a tincidunt velit commodo. Proin sem nibh, mollis venenatis risus lacinia, laoreet scelerisque arcu. Cras nec enim ut lacus egestas cursus. Sed eu sapien enim. Nam consectetur egestas justo. Integer nisi nisi, ultrices eu lobortis id, blandit vitae lectus. Quisque vitae quam scelerisque, semper ligula nec, lacinia nisl.'

const thinking = ref(false)
const question = ref('')
const messages = ref<Message[]>([])
const animatedMessage = ref<Message>({
  message: '',
  profileIcon: 'mdi-robot-excited',
  color: 'botMessage'
})
const currentMessage = ref('')

const typingInterval = ref<any>(null)

async function think() {
  thinking.value = true
  await new Promise((resolve) =>
    setTimeout(() => {
      thinking.value = false
      resolve(true)
    }, 500)
  )
}

function animate(text: string, delay: number) {
  let currentIndex = 0

  typingInterval.value = setInterval(() => {
    if (currentIndex === text.length) {
      clearInterval(typingInterval.value)
    } else {
      animatedMessage.value.message += text[currentIndex]
      currentIndex++
    }

    // Always scroll the user to the bottom of the screen while typing
    document.getElementById('animated-block')?.scrollIntoView()
  }, delay)
}

function sendQuestion() {
  messages.value.push({
    message: question.value,
    profileIcon: 'mdi-account',
    color: 'userMesssage'
  })
  question.value = ''
}

async function send() {
  if (!question.value) return

  if (currentMessage.value) {
    messages.value.push({
      message: currentMessage.value,
      profileIcon: 'mdi-robot-excited',
      color: 'botMessage'
    })
    animatedMessage.value.message = ''
    currentMessage.value = ''
    clearInterval(typingInterval.value)

    sendQuestion()
  } else {
    sendQuestion()
  }

  currentMessage.value = testText

  await think()
  animate(currentMessage.value, 30)
}
</script>

<style scoped>
.message-box {
  position: fixed;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  width: 100%;
  max-width: 1000px;
}
</style>
