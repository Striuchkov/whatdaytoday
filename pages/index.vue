<template>
  <main>
    <section class="self-center flex flex-col flex-1 items-center justify-center">
      <h1 class="title text-center">What day is it today?</h1>
      <h2 class="subtitle max-w-5xl mx-auto text-center">Today is {{formatedDate}}</h2>
      <span class="flex flex-row">
        <p class="m-3 text-center">Day of the year: {{daysSinceStart}}</p>
        <p class="m-3 text-center">Week of the year: {{weeksSinceStart}}</p>
      </span>
    </section>

    <section class="mt-8 card">
          <span class="flex-1">
            <h2 class="subtitle text-center">Calendar</h2>
          </span>
    </section>

    <section class="mt-8">
      <posts post-type="holidays" :amount="10" />
    </section>
    <section class="mt-8">
      <Subscribe />
    </section>
  </main>
</template>

<script>
import Subscribe from '~/components/global/Subscribe.vue'

export default {
  components: {
    Subscribe
  },
  data() {
    return {
      date: new Date(),
      currentYearStart: '',
      options: { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' },
      formatedDate: '',
      daysSinceStart: '',
      weeksSinceStart: ''
    }
  },
   async mounted() {
        this.formatedDate = this.date.toLocaleDateString("en-US", this.options);
        this.currentYearStart = new Date(this.date.getFullYear(), 0, 0);
        this.daysSinceStart = Math.floor((this.date - this.currentYearStart) / 1000 / 60 / 60 / 24);
        this.weeksSinceStart = Math.ceil(this.daysSinceStart / 7);
    },
  async asyncData({ $content, error }) {
    let posts;
    try {
      posts = await $content("blog").fetch();
    } catch (e) {
      error({ message: "Blog posts not found" });
    }
    return { posts };
  }  
}
</script>