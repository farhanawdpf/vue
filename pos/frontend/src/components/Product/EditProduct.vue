<template>
    <div class="container-fluid pt-4 px-4">
        <div class="row g-4">
            <div class="col-sm-12 col-xl-12">
                <div class="bg-light rounded h-100 p-4">
                    <h6 class="mb-4">Product Update Form</h6>

                    <div class="mb-3">
                        <label for="exampleInputEmail1" class="form-label">Category Name</label>
                        <select v-model="category_id" class="form-select mb-3" aria-label="Default select example">
                            <option value="0">Choose Category Name</option>
                            <option v-for="(d, i) in categoryList" :key="i" :value="d.id">{{ d.name }}</option>
                        </select>

                        <select v-model="sub_category_id" class="form-select mb-3" aria-label="Default select example">
                            <option value="0">Choose Sub Category Name</option>
                            <option v-for="(d, i) in sub_categoryList" :key="i" :value="d.id">{{ d.name }}</option>
                        </select>

                        <select v-model="brand_id" class="form-select mb-3" aria-label="Default select example">
                            <option value="0">Choose Brand Name</option>
                            <option v-for="(d, i) in brandList" :key="i" :value="d.id">{{ d.name }}</option>
                        </select>
                        <label for="exampleInputEmail1" class="form-label">Product Name</label>
                        <input type="text" class="form-control" v-model="name" id="exampleInputEmail1"
                            aria-describedby="emailHelp">
                    </div>
                    <!-- <button @click="save()" class="btn btn-primary mt-2 active">Save</button> -->
                    <button @click="updateProduct" class="btn btn-primary">Update</button> 

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
            categoryList: [],
            sub_categoryList: [],
            brandList: [],
            name: '',
            category_id: 0,
            sub_category_id: 0,
            brand_id: 0,
            id:this.$route.params.id,
            url:"http://127.0.0.1:8000/api/part1/product",
        }
    },
    methods: {
        getCategory() {
            axios.get("http://127.0.0.1:8000/api/part1/category")
                .then((res) => {
                    this.categoryList = res.data.data
                })
        },
        getSubCategory() {
            axios.get("http://127.0.0.1:8000/api/part1/sub_category")
                .then((res) => {
                    this.sub_categoryList = res.data.data
                })
        },
        getBrand() {
            axios.get("http://127.0.0.1:8000/api/part1/brand")
                .then((res) => {
                    this.brandList = res.data.data
                    // console.log(res.data.data)
                })
        },
        getproduct() {
            const id= this.id
            axios.get(this.url+'/'+this.id+'/edit')
                .then((res) => {
                    const dt= res.data.data;
                    this.sub_category_id=dt.sub_category_id;
                    this.category_id=dt.category_id;
                    this.brand_id=dt.brand_id;
                    this.name=dt.name;
                })
        },
        updateProduct() {
            const alldata={ name: this.name, category_id: this.category_id, sub_category_id: this.sub_category_id, brand_id: this.brand_id };
            axios.put(`${this.url}/${this.id}`,alldata)
                .then((response) => {
                this.$router.push({ name: 'product' })
                });
        },
    },
    mounted() {
        this.getCategory()
        this.getSubCategory()
        this.getBrand()
        this.getproduct()
    }
}
</script>