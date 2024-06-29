<template>
    <div class="container">
        <div class="row">
            <div class="col-12">
                <div class="row">
                    <div class="col-12 text-sm-center text-lg-start">
                        <h4 class="text-danger mb-4">ADD NEW STUDENT +</h4>
                    </div>
                    <div class="col-12">
                        <form @submit.prevent="DataInsert">
                            <div class="row">
                                <div class="col-12">
                                    <div class="form-group">
                                        <input class="form-control mb-2" type="text" placeholder="Student Name" required v-model="student.name"/>
                                        <input class="form-control mb-2" type="number" placeholder="Student Age" required v-model="student.age"/>
                                        <input type="file" accept="image/jpg, image/jpeg, image/png" name="image" id="image" class="d-none" @change="imageUpload"/>
                                        <label class="btn btn-success mt-4 col-12" for="image">Select an Image</label>
                                        <button type="submit" class="btn btn-primary mt-4 col-12 col-md-6 offset-md-6 col-lg-4 offset-lg-8">Register</button>
                                    </div>
                                </div>
                            </div>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import axios from 'axios';

export default {
    data() {
        return {
            student: {
                name: "",
                age: "",
                image: null,
                status: 'active',
            },
        };
    },
    methods: {
        async DataInsert() {
            await axios.post(route("dashboard.store"), this.student, {
                headers: {
                    "Content-Type": "multipart/form-data",
                },
            });
            this.student.name = "";
            this.student.age = "";
        },
        imageUpload(event) {
            this.student.image = event.target.files[0];
       },
    },
};
</script>