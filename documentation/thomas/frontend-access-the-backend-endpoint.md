# Access the backend endpoint

When we going to invoke the backend endpoint, we need to ensure:

1. We are logged on 
2. We have a valid access token
3. We invoke the backend microservice endpoint using axios

Here is one example invokation of the backend microservice endpoint.

```javascript
...
import axios from "axios";
...
export default {
  name: "app",
  components: {
    Catalog
  },
  computed: {
    isAuthenticated() {
      return this.$store.state.user.isAuthenticated;
    }
  },
  ...
  methods: {
   readProducts(categoryId, categoryName) {
      console.log("--> log readProducts.categoryId:  ", categoryId);
      var logURL= this.apiUrlProducts + "/" + categoryId + "/products";
      console.log("--> log readProducts.categoryId:  ", categoryId);
      console.log("--> log readProducts: URL ", logURL);
      this.categoryName = categoryName;
      if (this.loadingProducts == false) {
        this.loadingProducts = true;
        const axiosService = axios.create({
          timeout: 30000,
          headers: {
            "Content-Type": "application/json",
            Authorization: "Bearer " + this.$store.state.user.accessToken
          }
        });
        let that = this;
        axiosService
        .get(this.apiUrlProducts + "/" + categoryId + "/products")
        .then(function(response) {
          console.log("--> log: Product data : " + JSON.stringify(response.data));
          that.loadingProducts = false;
          that.error = "";
          that.$store.commit("addProducts", response.data);
        })
        .catch(function(e) {
            var error="--> log: Can't load products: " + e ;
            that.loadingProducts = false;
            console.error(error);
            that.errorLoadingProducts = error;
            that.$store.commit("logout");
        });
      }
    },
  },
```
