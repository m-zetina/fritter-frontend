<template>
<section>
    <header>
        <div class="left">
          <h2>
            Freet Feed
            <span v-if="$store.state.filter">
              by @{{ $store.state.filter }}
            </span>
          </h2>
        </div>
        <div class="right">
          <ChangeActiveFilterForm
            ref="changeActiveFilterForm"
            value=""
            placeholder="🔍 Filter by tags and authors (optional)"
            button="🔄 Change Active Filter"
          />
        </div>
    </header>
    <section
      v-if="$store.state.freets.length"
    >
        <FreetComponent
            v-for="freet in $store.state.freets"
            :key="freet.id"
            :freet="freet"
        />
    </section>
</section>
</template>

<script>
import FreetComponent from '@/components/Freet/FreetComponent.vue';
import ChangeActiveFilterForm from '@/components/Feed/ChangeActiveFilterForm.vue';

export default {
    name: 'FeedComponent',
    components: {ChangeActiveFilterForm, FreetComponent},
    // props: {
    //     // Data from the stored feed
    //     feed: {
    //         type: Object,
    //         required: true
    //     }
    // }
    methods: {
        async refresh() {
            const url = 'api/feeds';

            try {
                const r = await fetch(url);
                const res = await r.json();
                if (!r.ok) {
                    throw new Error(res.error);
                }
                this.$set(this.alerts, params.message, 'success');
                setTimeout(() => this.$delete(this.alerts, params.message), 3000);
            } catch (e) {
                this.$set(this.alerts, e, 'error');
                setTimeout(() => this.$delete(this.alerts, e), 3000);
            }
        }
    }   
}
</script>

<style scoped>
section {
  display: flex;
  flex-direction: column;
}

header, header > * {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

button {
    margin-right: 10px;
}

section .scrollbox {
  flex: 1 0 50vh;
  padding: 3%;
  overflow-y: scroll;
}
</style>