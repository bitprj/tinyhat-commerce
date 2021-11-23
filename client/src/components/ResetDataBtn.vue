<template>
    <label>
        <input id="fileUpload" type="file" @change="chooseFile" />
    </label>
</template>

<script>
import { mapActions, mapGetters } from 'vuex';
import Product from '@/components/Product';

export default {
    props: {
        products: {
            type: Array,
            required: false,
            defaultValue: () => []
        }
    },

    components: {
        Product
    },

    computed: {
        ...mapGetters({ cartItems: 'cart/getItems' })
    },

    methods: {
        ...mapActions({
            reset: 'products/reset'
        }),

        async resetData() {
            try {
                await this.reset();
            } catch (error) {
                console.error(error);
            }
        },

        async chooseFile() {
            try {
                console.log('code running');
                const file = document.getElementById('fileUpload').files[0];
                let formData = new FormData();
                formData.append('file', file);
                let options = {
                    method: 'POST',
                    body: formData
                };

                fetch('http://api.tinyhat.me/', options)
                    .then(resp => resp.json())
                    .then(data => {
                        console.log(data);
                    });
            } catch (error) {
                console.error(error);
            }
        }
    }
};
</script>
