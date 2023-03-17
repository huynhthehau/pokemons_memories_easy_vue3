<template>
   <div class="copyright">
    <p>
        This game owned by HTH in Vue 3 -
        <a href="#">view here</a>
    </p>
   </div>
</template>
<style>
.copyright {
  position: fixed;
  left: 50%;
  transform: translateX(-50%);
  bottom: 1.5rem;
  color: var(--light);
  z-index: 3;
  font-size: 1.5rem;
}
.copyright a {
  color: #f4dc26;
}
</style>