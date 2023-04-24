<template>
    <div class="container">
        <div class="card">
           <div class="card-header">
            <h4>Students</h4>  
            <RouterLink to="/students/create" class="btn btn-primary float-end">
                Add Student
            </RouterLink>
           </div> 
           <div class="card-body">
            <table class="table table-bordered">
                <thead>
                    <tr>
                        <th>ID</th>
                        <th>Name</th>
                        <th>Course</th>
                        <th>Email</th>
                        <th>Phone</th>
                        <th>Created At</th>
                        <th>Action</th>
                    </tr>
                </thead>
                <tbody v-if="this.students.length > 0">
                    <tr v-for="(student,index) in this.students" :key="index">
                        <td>{{ student.id }}</td>
                        <td>{{ student.name }}</td>
                        <td>{{ student.course }}</td>
                        <td>{{ student.email }}</td>
                        <td>{{ student.phone }}</td>
                        <td>{{ student.created_at }}</td>
                        <td>
                            <RouterLink :to="{path: '/students/'+student.id+'/edit'}" class="btn btn-success mx-1">
                                Edit
                            </RouterLink>
                            <button type="button" @click="deleteStudent(student.id)" class="btn btn-danger mx-1">
                                Delete
                            </button>
                        </td>
                    </tr>
                </tbody>
                <tbody v-else>
                    <tr colspan="7">Loading...</tr>
                </tbody>
            </table>
           </div>
        </div>     
    </div>
  </template>
  
  <script>
    import axios from 'axios'

    export default{
        name:'students',
        data(){
            return{
                students:[]
            }
        },
        mounted(){
         this.getStudent();
        },
        methods:{
            getStudent(){
                axios.get('http://localhost:8000/api/students').then(res =>{
                    this.students = res.data.students
                    //console.log(res)
                });
            },
            deleteStudent(studentId){
                if(confirm('Yakin Hapus Data Ini ?')){
                    axios.delete(`http://localhost:8000/api/students/${studentId}/delete`)
                        .then(res => {
                            alert(res.data.message);
                            this.getStudent();
                        })
                        .catch(function (error){
                            if(error.response){
                                if(error.response.status == 404){
                                    alert(error.response.data.message);
                                }
                            }
                        });
                }
            }
        }
    }
  </script>