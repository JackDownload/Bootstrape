<template>
  <div class="has-background-primary-off">
    <Nav color="has-background-primary" />
    <div class="fullscreen">
      <article class="section max-width-78ch">
        <!-- TITLE -->
        <h1 class="section-title">Blog Recipes</h1>
        <!-- CONTAINER FOR PROJECTS -->
        <div class="columns is-multiline">
          <div
            v-for="(recipe, index) in recipes"
            :key="index"
            class="column is-4"
          >
            <RecipeCard v-bind="recipe" background="has-background-primary" />
          </div>
        </div>
      </article>
    </div>
    <Footer />
  </div>
</template>

<script>
import getSiteMeta from "~/utils/getSiteMeta.js";
import { TweenMax, Power3 } from "gsap";

export default {
  transition: {
    mode: "out-in",
    css: false,
    enter() {
      TweenMax.fromTo(
        ".section-title",
        { x: "10%" },
        { x: "0%", duration: 0.1 }
      );
      TweenMax.fromTo(
        ".columns",
        { x: "10%", autoAlpha: 0 },
        {
          x: "0%",
          autoAlpha: 1,
          delay: 0.25,
          duration: 0.5,
          ease: Power3.easeOut,
        }
      );
    },
  },
  computed: {
    meta() {
      const metaData = {
        url: `${this.$config.baseURL}/recipe`,
      };
      return getSiteMeta(metaData);
    },
  },
  head() {
    return {
      title: "Recipe | Connor Rothschild",
      meta: [...this.meta],
      link: [
        {
          hid: "canonical",
          rel: "canonical",
          href: `${this.$config.baseURL}/recipe`,
        },
      ],
    };
  },
  async asyncData({ $content, params }) {
    const recipes = await $content("recipes", params)
      .sortBy("date", "desc")
      .fetch();

    return {
      recipes,
    };
  },
};
</script>

<style scoped>
p {
  font-size: 1rem !important;
}
</style>
