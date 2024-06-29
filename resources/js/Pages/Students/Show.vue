<template>
    <div class="container">
        <div class="row">
            <div class="col-12">
                <div class="row">
                    <div class="col-12 text-sm-center text-lg-start">
                        <h4 class="text-danger mb-4">VIEW ALL STUDENTS *</h4>
                    </div>
                    <div class="col-12">
                        <table class="table text-center">
                            <thead>
                                <tr>
                                <th scope="col">*</th>   
                                <th scope="col">ID</th>
                                <th scope="col">Image</th>
                                <th scope="col">Name</th>
                                <th scope="col">Age</th>
                                <th scope="col">Status</th>
                                <th scope="col">Update</th>
                                <th scope="col">Delete</th>
                            </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(student, key) in allStudents">
                                    <td scope="row">{{ ++key }}</td>
                                    <td>{{ student.id }}</td>
                                    <td>
                                        <img :src="'images/' + student.image" width="100px" height="100px"/>
                                    </td>
                                    <td>{{ student.name }}</td>
                                    <td>{{ student.age }}</td>
                                    <td>
                                        <button v-if="student.status == 'active'" class="btn btn-success" @click.prevent="statusChange(student.id)">Active</button>
                                        <button v-else class="btn btn-danger" @click.prevent="statusChange(student.id)">Inactive</button>
                                    </td>
                                    <td>
                                        <button class="btn btn-warning" @click.prevent="editStudent(student.id)">Update</button>
                                    </td>
                                    <td>
                                        <button class="btn btn-danger" @click.prevent="deleteStudent(student.id)">Delete</button>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <div class="modal fade" id="studentEdit" data-bs-backdrop="static" data-bs-keyboard="false" tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h1 class="modal-title fs-5" id="staticBackdropLabel">Edit Student Details</h1>
                </div>
                <div class="modal-body" id="taskEditContent">
                    <form @submit.prevent="updateStudent">
                        <div class="row">
                            <div class="col-6 offset-3">
                                <div class="form-group">
                                    <input class="form-control" type="text" name="name" v-model="studentUpdate.name" placeholder="Student Name" aria-label="default input example" required/>
                                    <input class="form-control mt-4" type="number" name="age" aria-label="default input example" v-model="studentUpdate.age" required placeholder="Student Age"/>
                                    <button class="btn btn-primary col-12 mt-2" type="submit">Update Data</button>
                                </div>
                            </div>
                        </div>
                    </form>
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
            allStudents: [],
            studentUpdate: {
                name: '',
                age: '',
            },
        };
    },
    beforeMount() {
        this.getStudents();
    },
    methods:{
        async getStudents() {   
            const students = (await axios.get(route("dashboard.list"))).data;
            this.allStudents = students;
        },
        async deleteStudent(id) {
            await axios.delete(route("dashboard.delete", id));
            this.getStudents();
        },
        async statusChange(id) {
            await axios.get(route("dashboard.statusChange", id));
            this.getStudents();
        },
        async editStudent(id) {
            const student = (await axios.get(route("dashboard.get", id))).data;
            this.studentUpdate = student;
            $("#studentEdit").modal("show");
        },
        async updateStudent() {
            await axios.post(
                route("dashboard.update", this.studentUpdate.id),
                this.studentUpdate
            );
            this.getStudents();
            $("#studentEdit").modal("hide");
        },
    }
}
</script>