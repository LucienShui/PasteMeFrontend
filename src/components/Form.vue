<template>
    <b-row>
        <b-col md="1" lg="2"></b-col>
        <b-col md="10" lg="8">
            <p>跳转中</p>
        </b-col>
        <b-col md="1" lg="2"></b-col>
    </b-row>
</template>

<script>
    import stateMixins from "../assets/js/mixins/stateMixin";
    export default {
        name: "Form",
        mixins: [stateMixins],
        mounted() {
            location.href = '/'
        },
        data() {
            return {
                form: {
                    lang: 'plain',
                    content: null,
                    password: null,
                },
                read_once: []
            }
        },
        methods: {
            onSubmit() {
                let key = "";
                if (this.$route.params.key !== '') {
                    key = this.$route.params.key;
                } else if (this.read_once.length > 0) {
                    key = "once"
                }
                const sendArgs = [`${this.$store.getters.config.api.backend}${key}`, this.form];
                const sendFunc = key === "" || key === "once" ? this.api.post : this.api.put;
                sendFunc(...sendArgs).then(response => {
                    if (response.status === 201) {
                        this.updateView("success");
                        this.updateKey(response.key);
                    }
                });
            }
        }
    }
</script>

<style scoped>

</style>
