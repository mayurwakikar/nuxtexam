<template>
<div>
    <title>
        Productlist Project
    </title>
    <h1>Form To Add Products</h1>
    <div>
        <form action="">
            <div>
                <label for="pname">Product Name</label>
                <input type="text" v-model="formdata.pname" name="pname" id="pname">
            </div>
            <div>
                <label for="price">Product Price</label>
                <input type="text" v-model="formdata.price" name="pname" id="price">
            </div>
            <div>
                <label for="category">Product Category</label>
                <input type="text" v-model="formdata.category" name="pname" id="">
            </div>
            <div>
                <label for="color">Product color</label>
                <input type="text" v-model="formdata.color" name="pname" id="">
            </div>
            <div>
                <button type="submit" @click="addpro">Add Product</button>
                <button type="reset">Reset</button>
            </div>
        </form>
        <table border="2">
            <tr>
                <td>Product ID</td>
                <td>Product Name </td>
                <td>Product Price</td>
                <td>Product Category</td>
                <td>Product Color</td>
                <td>Delete</td>
                <td>Edit Data</td>
            </tr>
            <tr v-for="(item,i) in myarr" :key="item">
                <td>{{item.id = i + 1}}</td>
                <td>{{item.pname}}</td>
                <td>{{item.price}}</td>
                <td>{{item.category}}</td>
                <td>{{item.color}}</td>
                <td><button @click="deletepro(i)">Delete Product</button></td>
                <td><button @click="editpro(i)">Edit</button></td>
            </tr>
        </table>
        <div>
            <label for="searchpro">Serach Product</label>
            <input type="text" name="searchpro" id="searchpro">
            <button type="search" @click="searchpro">Search</button>
        </div>
    </div>
</div>
</template>

<script>
export default {
    name: "product-data",
    data() {
        return {
            isEdit: false,
            indexEdit: -1,
            myarr: [],
            formdata: {
                id: 0,
                pname: "",
                price : "",
                category : "",
                color : "",
            }
        }
    },
    methods : {
         addpro(event){
            event.preventDefault();
            if(this.isEdit == true)
            {
              this.myarr[this.indexEdit] = this.formdata;
              this.isEdit = false;
              this.indexEdit = -1;
            }
            else
            {
              this.myarr.push(this.formdata);
            }
            this.formdata = {
                id: 0,
                pname: "",
                price : "",
                category : "",
                color : "",
            }
            console.log("Formdata is :", this.myarr);
         },
         deletepro(index) {
            this.myarr.splice(index, 1);
        },
         editpro(index){
          this.formdata.pname = this.myarr[index].pname;
          this.formdata.price = this.myarr[index].price;
          this.formdata.category = this.myarr[index].category;
          this.formdata.color = this.myarr[index].color;
          this.isEdit = true;
          this.indexEdit = index ;
        },
        userpro(userName){
            // user1 = this.userName
            // if(this.allUserData.filter(e => e.firstName == userName})){
            //     alert("user Found" + e.firstName+ ""+e.lastName);
            // }
            console.log(userName);
            this.userFound = this.myarr.filter((e) => {
                if(e.pname == userName){
                    // this.userFound.push(e);
                    console.log(e);
                    return e;
                    // alert("user Found" + e.firstName+ ""+e.lastName);
                }
                // else{
                // alert("user not found");
                // }
                // console.log(this.userFind);
            });
            // console.log(find1);
            console.log(this.userFound);
        },
    }
}
</script>
