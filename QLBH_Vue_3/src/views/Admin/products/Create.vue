<template lang="">
  <div class="create py-5">
    <div class="container">
      <h3 class="text-center text-4xl font-semibold mb-4">Create</h3>
      <div class="crad px-5">
        <div class="crad-body">
          <form @submit.prevent="addProduct">
            <div class="grid grid-cols-4 gap-4">
              <div class="w-full">
                <div class="form-group mb-3">
                  <label class="block text-sm font-medium leading-6 text-gray-900">Product name</label>
                  <input
                    type="text"
                    class="block w-full rounded-md border border-gray-100 py-1.5 text-gray-900 shadow-sm placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600"
                    placeholder="Enter name product"
                    v-model="product.name"
                  />
                </div>
              </div>
              <div class="w-full">
                <div class="form-group mb-3">
                  <label class="block text-sm font-medium leading-6 text-gray-900">Category</label>
                  <select class="block w-full rounded-md border border-gray-100 py-1.5 text-gray-900 shadow-sm focus:ring-2 focus:ring-inset focus:ring-indigo-600" v-model="product.category">
                    <option
                      v-for="(value, key) in categories"
                      :key="key"
                      :value="key"
                    >
                      {{ value }}
                    </option>
                  </select>
                </div>
              </div>
              <div class="w-full">
                <div class="form-group mb-3">
                  <label class="block text-sm font-medium leading-6 text-gray-900">Price ($)</label>
                  <input
                    type="number"
                    class="block w-full rounded-md border border-gray-100 py-1.5 text-gray-900 shadow-sm placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
                    v-model="product.price"
                  />
                </div>
              </div>
              <div class="col-span-auto">
                <div class="form-group mb-3">
                  <label class="block text-sm font-medium leading-6 text-gray-900">Color</label>
                  <input
                    type="color"
                    class="p-1 h-10 w-full block bg-white border border-gray-100 cursor-pointer rounded-lg disabled:opacity-50 disabled:pointer-events-none dark:bg-slate-900 dark:border-gray-700"
                    v-model="product.color"
                  />
                </div>
              </div>
              <div class="col-span-3">
                <div class="form-group mb-3">
                  <label class="block text-sm font-medium leading-6 text-gray-900">Description</label>
                  <textarea
                    cols=""
                    rows="10"
                    class="block w-full rounded-md border border-gray-100 py-1.5 text-gray-900 shadow-sm placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
                    placeholder="Enter description"
                    v-model="product.desc"
                  ></textarea>
                </div>
              </div>
              <div class="col-span-1">
                <label for="cover-photo" class="block text-sm font-medium leading-6 text-gray-900">Cover photo</label>
                <div class="flex justify-center rounded-lg border border-dashed border-gray-900/25 px-6 py-10 shadow-sm">
                  <div class="text-center">
                    <svg class="mx-auto h-14 w-12 text-gray-300" viewBox="0 0 24 24" fill="currentColor" aria-hidden="true">
                      <path fill-rule="evenodd" d="M1.5 6a2.25 2.25 0 012.25-2.25h16.5A2.25 2.25 0 0122.5 6v12a2.25 2.25 0 01-2.25 2.25H3.75A2.25 2.25 0 011.5 18V6zM3 16.06V18c0 .414.336.75.75.75h16.5A.75.75 0 0021 18v-1.94l-2.69-2.689a1.5 1.5 0 00-2.12 0l-.88.879.97.97a.75.75 0 11-1.06 1.06l-5.16-5.159a1.5 1.5 0 00-2.12 0L3 16.061zm10.125-7.81a1.125 1.125 0 112.25 0 1.125 1.125 0 01-2.25 0z" clip-rule="evenodd" />
                    </svg>
                    <div class="mt-3 flex text-sm leading-6 text-gray-600">
                      <label for="file-upload" class="relative cursor-pointer rounded-md bg-white font-semibold text-indigo-600 focus-within:outline-none focus-within:ring-2 focus-within:ring-indigo-600 focus-within:ring-offset-2 hover:text-indigo-500">
                        <span>Upload a file</span>
                        <input id="file-upload" type="file" @change="handleFileUpLoad" class="sr-only">
                      </label>
                      <p class="pl-1">or drag and drop</p>
                    </div>
                    <p class="text-xs leading-5 text-gray-600">PNG, JPG, GIF up to 10MB</p>
                    <div class="text-center h-14"><img width="80px" class="img-fluid mx-auto" v-if="imageUrl" :src="imageUrl" /></div>
                  </div>
                </div>
              </div>
              <div class="col-span-full">
                <div class="text-center">
                  <button type="submit" class="w-full inline-flex justify-center rounded-md border border-transparent shadow-sm px-4 py-2 bg-blue-600 text-base font-medium text-white hover:bg-blue-700 hover:text-white focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500 sm:ml-3 sm:w-auto sm:text-sm">
                    Add product
                  </button>
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
import { ref } from "vue";
import ProductService from "@/services/ProductService";
import router from "@/router";
import { notify } from "notiwind"
import {
  ref as firebaseRef,
  getDownloadURL,
  uploadBytes,
} from "firebase/storage";
import { storage } from "@/firebase";

export default {
  setup() {
    const selectFile = ref(null);
    const product = ref({});
    const categories = ref({
      mobile: "Mobile",
      laptop: "Laptop",
      apple_watch: "Apple Watch",
      iphone: "iPhone",
      oppo: "Oppo",
      android: "Android",
      ios: "IOS",
    });
    const imageUrl = ref(null); 

    const handleFileUpLoad = (e) => {
      const file = e.target.files[0];
      selectFile.value = file;

      const reader = new FileReader();
      reader.onload = (event) => {
        imageUrl.value = event.target.result;
      };
      reader.readAsDataURL(file);
    };

    const addProduct = async () => {
      if (!product.value) {
        console.log("Product is undefined or null");
        return;
      }
      try {
        if (selectFile.value) {
          const timestamp = Date.now();
          //rename image 
          const uniqueFileName = `${timestamp}_${selectFile.value.name}`;
          const storageRef = firebaseRef(storage,"images/" + uniqueFileName); // lưu đường dẫn
          await uploadBytes(storageRef, selectFile.value);
          console.log("Upload file successfully");
          const imageUrl = await getDownloadURL(storageRef);
          product.value.url_image = imageUrl;
        }
        await ProductService.create(product.value);
        console.log("Created new item successfully!");
        notify({
          group: "foo",
          title: "Create",
          position: "top-center", 
          type: "success",
          text: "Created new item successfully"
        }, 3000);
        router.push('/list-product');
      } catch (error) {
        console.error(error);
      }
    };

    return {
      product,
      categories,
      addProduct,
      handleFileUpLoad,
      imageUrl
    };
  },
};
</script>
<style lang=""></style>
