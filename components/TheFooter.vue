<template>
  <footer>
    <div class="flex justify-center py-3">
      &copy; {{ new Date().getFullYear() }} Eclectic Saddlebag
    </div>
    <div class="flex justify-center pb-4 mx-1 social">
      <a href="https://github.com/eclectic-coding">
        <fa :icon="faGitHub" class="mx-2 text-3xl github" />
      </a>

      <a href="https://twitter.com/eclectic-coding">
        <fa :icon="faTwitterSquare" class="mx-2 text-3xl twitter" />
      </a>

      <a href="https://linkedin.com/eclectic-coding">
        <fa :icon="faLinkedin" class="mx-2 text-3xl linkedin" />
      </a>

      <a href="https://dev.to.com/eclectic-coding">
        <fa :icon="faDev" class="mx-2 text-3xl dev-to" />
      </a>
    </div>
  </footer>
</template>

<script>
import {
  faGithub,
  faTwitterSquare,
  faLinkedin
} from '@fortawesome/fontawesome-free-brands'
import { faDev } from '@fortawesome/free-brands-svg-icons'

export default {
  computed: {
    faGitHub() {
      return faGithub
    },
    faTwitterSquare() {
      return faTwitterSquare
    },
    faLinkedin() {
      return faLinkedin
    },
    faDev() {
      return faDev
    }
  }
}
</script>

<style lang="scss">
footer {
  color: #35495e;
}
</style>
