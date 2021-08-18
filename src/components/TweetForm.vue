<template>
  <div class="tweet-form container" :class="{ open: showForm }">
      <form @submit.prevent="sendTweet">
          <input class="form-control" placeholder="Tu nombre" type="text" v-model="username">
          <input class="form-control" rows="3" placeholder="Escribe tu tweet" type="textarea" v-model="tweet">
          <button type="submit" class="btn btn-primary">Enviar Tweet</button>
      </form>
  </div>
</template>

<script>
import { ref } from "vue";
import { saveTweetApi } from "../api/tweet";

export default {
    props: {
        showForm: Boolean,
        reloadTweets: Function,
        openCloseForm: Function,
    },
    setup(props) {
        let username = ref("");
        let tweet = ref("");

        const sendTweet = () => {
            // console.log("Enviando formulario");
            // console.log(username.value);
            // console.log(tweet.value);
            if(!tweet.value || !username.value) return;
            saveTweetApi(tweet.value, username.value);
            // Dejamos los campos vacíos.
            tweet.value = "";
            username.value = "";
            // Recargamos los tweets.
            props.reloadTweets();
            // Y cerramos el formulario.
            props.openCloseForm();
        };
        return {
            sendTweet,
            username,
            tweet,
        }
    }
}
</script>

<style lang="scss" scoped>

.tweet-form {
    margin-top: 20px;
    height: 0;
    overflow: hidden;

    &.open {
        height: auto;
    }

    form {
        margin-bottom: 50px;
        
        input {
            margin-bottom: 10px;

            button {
                width: 100%;
                margin-top: 10px;;
            }
        }
    }
}

</style>