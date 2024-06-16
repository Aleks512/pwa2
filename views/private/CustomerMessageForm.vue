<template>
  <div>
    <h2>Envoyer un message</h2>
    <form @submit.prevent="sendMessage">
      <div>
        <label for="receiver_email">Email du destinataire</label>
        <input type="email" v-model="message.receiver_email" id="receiver_email" required>
      </div>
      <div>
        <label for="content">Message</label>
        <textarea v-model="message.content" id="content" required></textarea>
      </div>
      <button type="submit">Envoyer</button>
    </form>
    <div v-if="successMessage">{{ successMessage }}</div>
    <div v-if="errorMessage">{{ errorMessage }}</div> <!-- Display error message -->
  </div>
</template>

<script>
import getAPI from '@/axios-api';
import { mapGetters } from 'vuex';

export default {
  data() {
    return {
      message: {
        receiver_email: '',
        content: ''
      },
      successMessage: '',
      errorMessage: '' // Error message state
    };
  },
  computed: {
    ...mapGetters({
      isLoggedIn: 'auth/isLoggedIn',
    }),
  },
  methods: {
    async sendMessage() {
      try {
        await getAPI.post('/customer-create-message/', this.message);
        this.successMessage = 'Message envoyé avec succès!';
        this.errorMessage = ''; // Clear error message
        this.message.receiver_email = '';
        this.message.content = '';
      } catch (error) {
        console.error('Error sending message:', error);
        this.errorMessage = 'Erreur lors de l\'envoi du message';
      }
    }
  }
};
</script>
