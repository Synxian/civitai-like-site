<template>
    <header class="header">
      <div class="w-4">
        <img src="../assets/gifs/felix.gif" alt="felix the cat" class="animate-moveAcross">
      </div>
    </header>
  </template>

<style scoped>
.header {
  position: relative;
  width: 100%;
  height: 100px; /* Adjust based on your header height */
  overflow: hidden;
  background-color: #f0f0f0; /* Optional background color */
}

.gif-container {
  position: absolute;
  width: 100%;
  height: 100%;
}

.moving-gif {
  height: 80px; /* Adjust based on your GIF size */
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  animation: moveAcross 10s linear infinite; /* Adjust duration as needed */
}

@keyframes moveAcross {
  0% {
    left: -100px; /* Start off-screen to the left */
  }
  100% {
    left: 100%; /* End off-screen to the right */
  }
}
</style>