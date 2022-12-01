<!-- Form for getting freets (all, from user) (inline style) -->

<script>
import InlineForm from '@/components/common/InlineForm.vue';

export default {
  name: 'ChangeActiveFilterForm',
  mixins: [InlineForm],
  methods: {
    async submit() {
      console.log('val', this.value)
      const activeFilter = this.value ? this.value : 'latest';
      const message = `Successfully changed active filter to ${activeFilter}.`
      const options = {
        method: 'PATCH',
        headers: {'Content-Type': 'application/json'},
        body: JSON.stringify({activeFilter}),
        callback: () => {
          this.$set(this.alerts, message, 'success');
          setTimeout(() => this.$delete(this.alerts, message), 3000);
        }
      }

      try {
        const r = await fetch('/api/feeds', options);
        const res = await r.json();
        if (!r.ok) {
          throw new Error(res.error);
        }
        options.callback();
      } catch (e) {
        this.$set(this.alerts, e, 'error');
        setTimeout(() => this.$delete(this.alerts, e), 3000);
      }
    }
  }
};
</script>
