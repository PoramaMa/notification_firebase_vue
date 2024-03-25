<template>
  <h1>Notification</h1>
</template>
<script setup>
import { initializeApp } from "firebase/app";
import { getMessaging, getToken, onMessage } from "firebase/messaging";

const firebaseConfig = {
  apiKey: "AIzaSyDqpw5UbHOKejed7uZdFaVYGaEbVUmFjYw",
  authDomain: "testnoti-709f4.firebaseapp.com",
  projectId: "testnoti-709f4",
  storageBucket: "testnoti-709f4.appspot.com",
  messagingSenderId: "873103653252",
  appId: "1:873103653252:web:c6103781dcb51ef33d45d1",
  measurementId: "G-RWD6KSFKJ8",
};

const app = initializeApp(firebaseConfig);
const messaging = getMessaging(app);
getToken(messaging, {
  vapidKey: process.env.VAPIDKEY,
})
  .then((currentToken) => {
    if (currentToken) {
      console.log("currentToken :: ", currentToken);
    } else {
      console.log(
        "No registration token available. Request permission to generate one."
      );
    }
  })
  .catch((err) => {
    console.log("An error occurred while retrieving token. ", err);
  });

function requestPermission() {
  console.log("Requesting permission...");
  Notification.requestPermission().then((permission) => {
    if (permission === "granted") {
      console.log("Notification permission granted.");
    }
  });
}
requestPermission();

onMessage(messaging, (payload) => {
  console.log("Message received. ", payload);
  new Notification("Example notification", {
    title: payload.notification.title,
    body: payload.notification.body,
  });
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style>
