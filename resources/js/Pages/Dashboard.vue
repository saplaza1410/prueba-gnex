<script setup>
import BreezeAuthenticatedLayout from '@/Layouts/Authenticated.vue';
import { Head } from '@inertiajs/inertia-vue3';
import { defineComponent } from '@vue/runtime-core';
import swal from 'sweetalert2';
window.Swal = swal;
</script>

<script>

import axios from 'axios';
export default defineComponent({
    data() {
        return {
            users : []
        }
    },
    methods:{
        getUser(){
            axios.get("/users")
            .then((response) => {
                this.users = response.data
            })
            .catch();
        },
        deleteUser(id){
           Swal.fire({
            title: 'Are you sure to delete the user?',
            showDenyButton: false,
            showCancelButton: true,
            }).then((result) => {
            /* Read more about isConfirmed, isDenied below */
            if (result.isConfirmed) {
                axios.delete("/users/destroy/"+id)
            .then(response => {
                console.log(response)
            })
            .catch(error => {console.log(error.response)});
            this.getUser();
                Swal.fire('User deleted successfully!', '', 'success')
            } else if (result.isDenied) {
                //Swal.fire('Changes are not saved', '', 'info')
            }
            })
        }
    },
    created(){
        this.getUser();
    }
})


</script>


<template>
    <Head title="Dashboard" />

    <BreezeAuthenticatedLayout>
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                Dashboard
            </h2>
        </template>

        <div class="py-12">
            <div  class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
                    <div class="p-6 bg-white border-b border-gray-200">

                        <div class="mt-8 bg-white dark:bg-gray-800 overflow-hidden shadow sm:rounded-lg">
                            <div class="grid grid-cols-1 md:grid-cols-2">
                                <div v-for="user in users" :key="user.id" class="p-6">
                                    <div class="flex items-center">
                                        <svg height="24" id="svg3171" version="1.1" viewBox="0 0 6.3499999 6.3500002" width="24" xmlns="http://www.w3.org/2000/svg" xmlns:cc="http://creativecommons.org/ns#" xmlns:dc="http://purl.org/dc/elements/1.1/" xmlns:inkscape="http://www.inkscape.org/namespaces/inkscape" xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" xmlns:sodipodi="http://sodipodi.sourceforge.net/DTD/sodipodi-0.dtd" xmlns:svg="http://www.w3.org/2000/svg"><defs id="defs3165"/><g id="layer1"><path d="m 3.176292,0.52916623 c -0.7275637,0 -1.3245961,0.59496497 -1.3245961,1.32252887 0,0.7275639 0.5970324,1.3245964 1.3245961,1.3245962 0.727564,0 1.3220123,-0.5970323 1.3220123,-1.3245962 0,-0.7275639 -0.5944483,-1.32252887 -1.3220123,-1.32252887 z M 2.1173387,3.4398668 c -0.7943443,0 -1.58817203,0.5292183 -1.58817203,1.3220123 V 5.350014 c 0.05042,0.3308308 0.2646094,0.4713351 0.52973543,0.4708184 H 5.2916149 C 5.5567406,5.8213615 5.731772,5.6000289 5.8208331,5.350014 V 4.7618791 C 5.8213623,3.7044758 5.0275223,3.4398668 4.2326614,3.4398668 Z" id="path1826" style="color:#000000;font-style:normal;font-variant:normal;font-weight:normal;font-stretch:normal;font-size:medium;line-height:normal;font-family:sans-serif;font-variant-ligatures:normal;font-variant-position:normal;font-variant-caps:normal;font-variant-numeric:normal;font-variant-alternates:normal;font-variant-east-asian:normal;font-feature-settings:normal;font-variation-settings:normal;text-indent:0;text-align:start;text-decoration:none;text-decoration-line:none;text-decoration-style:solid;text-decoration-color:#000000;letter-spacing:normal;word-spacing:normal;text-transform:none;writing-mode:lr-tb;direction:ltr;text-orientation:mixed;dominant-baseline:auto;baseline-shift:baseline;text-anchor:start;white-space:normal;shape-padding:0;shape-margin:0;inline-size:0;clip-rule:nonzero;display:inline;overflow:visible;visibility:visible;opacity:1;isolation:auto;mix-blend-mode:normal;color-interpolation:sRGB;color-interpolation-filters:linearRGB;solid-color:#000000;solid-opacity:1;vector-effect:none;fill:#000000;fill-opacity:1;fill-rule:nonzero;stroke:none;stroke-width:0.52922;stroke-linecap:round;stroke-linejoin:round;stroke-miterlimit:4;stroke-dasharray:none;stroke-dashoffset:0;stroke-opacity:1;color-rendering:auto;image-rendering:auto;shape-rendering:auto;text-rendering:auto;enable-background:accumulate;stop-color:#000000;stop-opacity:1"/></g></svg>
                                        <div class="ml-4 text-lg leading-7 font-semibold">{{user.name}}</div>
                                    </div>

                                    <div class="ml-12">
                                        <div class="mt-2 text-gray-60 dark:text-gray-400 text-sm">
                                            Email: {{user.email}}
                                        </div>
                                    </div>
                                    <div class="ml-12">
                                        <div class="mt-2 text-gray-60 dark:text-gray-400 text-sm">
                                            Phone: {{user.phone}}
                                        </div>
                                    </div>
                                    <form v-on:submit.prevent="deleteUser(user.id)" v-if="$page.props.auth.user.id != user.id">
                                        <div class="ml-12">
                                            <button type="submit">
                                            <svg height="24" id="Layer_1" version="1.1" data-name="Layer 1" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 105.16 122.88"><path class="cls-1" d="M11.17,37.16H94.65a8.4,8.4,0,0,1,2,.16,5.93,5.93,0,0,1,2.88,1.56,5.43,5.43,0,0,1,1.64,3.34,7.65,7.65,0,0,1-.06,1.44L94,117.31v0l0,.13,0,.28v0a7.06,7.06,0,0,1-.2.9v0l0,.06v0a5.89,5.89,0,0,1-5.47,4.07H17.32a6.17,6.17,0,0,1-1.25-.19,6.17,6.17,0,0,1-1.16-.48h0a6.18,6.18,0,0,1-3.08-4.88l-7-73.49a7.69,7.69,0,0,1-.06-1.66,5.37,5.37,0,0,1,1.63-3.29,6,6,0,0,1,3-1.58,8.94,8.94,0,0,1,1.79-.13ZM5.65,8.8H37.12V6h0a2.44,2.44,0,0,1,0-.27,6,6,0,0,1,1.76-4h0A6,6,0,0,1,43.09,0H62.46l.3,0a6,6,0,0,1,5.7,6V6h0V8.8h32l.39,0a4.7,4.7,0,0,1,4.31,4.43c0,.18,0,.32,0,.5v9.86a2.59,2.59,0,0,1-2.59,2.59H2.59A2.59,2.59,0,0,1,0,23.62V13.53H0a1.56,1.56,0,0,1,0-.31v0A4.72,4.72,0,0,1,3.88,8.88,10.4,10.4,0,0,1,5.65,8.8Zm42.1,52.7a4.77,4.77,0,0,1,9.49,0v37a4.77,4.77,0,0,1-9.49,0v-37Zm23.73-.2a4.58,4.58,0,0,1,5-4.06,4.47,4.47,0,0,1,4.51,4.46l-2,37a4.57,4.57,0,0,1-5,4.06,4.47,4.47,0,0,1-4.51-4.46l2-37ZM25,61.7a4.46,4.46,0,0,1,4.5-4.46,4.58,4.58,0,0,1,5,4.06l2,37a4.47,4.47,0,0,1-4.51,4.46,4.57,4.57,0,0,1-5-4.06l-2-37Z"/></svg>
                                            </button>
                                        </div>
                                    </form>

                                </div>


                            </div>
                        </div>

                    </div>
                </div>
            </div>
        </div>




    </BreezeAuthenticatedLayout>
</template>



