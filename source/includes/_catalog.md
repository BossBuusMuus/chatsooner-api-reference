# Catalog

<aside class="notice">
Because we are WhatsApp first platform calling this endpoint without a Facebook Catalog attached to your WABA will return a 406 Not acceptable error.
</aside>

You're required to create your own Facebook Catalog through Facebook's Commerce Manager. Currently there are no API ways to achieve this, however it may change in the future.

After creating the Facebook Catalog you can email [Support](mailto:onboarding@chatsooner.com) with the Catalog Name, Catalog ID and **2 catalog management links**.

For more details on this process you can read this article [here](https://chatsooner.com)
## Adding Products

> Use this to add a product:

```shell
# With shell, you can just pass the correct header with each request
curl --request POST \
  --url https:/api.chatsooner.com/add/store/product \
  --header 'Authorization: [API Key]' \
  --header 'Content-Type: multipart/form-data' \
  --form 'productName=My Product Name' \
  --form 'category=My Product Category' \
  --form 'description=My Description' \
  --form 'images=[File Objects]' \
  --form 'sku=My SKU' \
  --form 'websiteUrl=Product Website Url' \
  --form isActive=true \
  --form 'specifications=[{"specificationName":"specificationValue"}]' \
  --form "salePrice={from: 'Date and Timestamp', to:'Date and Timestamp', salePrice:'amount'}"
  --form 'currency=ZAR' \
  --form 'brand=My Brand' \
  --form checkout=false \
  --form collectEmail=false \
  --form collectPaymentMethod=false \
  --form collectPhysicalAddress=false \
  --form collectRecipientName=false
```

```javascript
//No javascript library available for this endpoint. Use Shell instead
```

Adding products with this endpoint adds them to your global store. This means if you have multiple bots across multiple channels, they will all share the same catalog.

### HTTPS Request

`POST https://api.chatsooner.com/add/store/product`

### Form Parameters

Parameter | Required | Description
--------- | ------- | -----------
productName | true | The title of the product
category | true | The category of the product
amount | true | The price of the product as a float
currency | true | ISO currency code
images | true | This is an array of images. This can be an array of image Urls or Javascript file objects. At least one image should be in the array.
productUrl | true | This is a the product's website url. If there isn't one you can put your website url
isActive | optional | Defaults to true. Products set to false do not show up on your chatbots
description | optional | The product description (recommended)
brand | optional | The brand name of the product (recommended)
sku | optional | Your sku
stock| optional |The amount of products available. Should stock be set to 0 the product will display Out of Stock.
salePrice | optional | This puts the product on a sale with a start date and end date.
specifications | optional | These are key value pairs that add more context to the product. Highly recommended for a better search and discovery process for your end users.
checkout | optional | This activates chatbot checkout for this product. To fully grasp the implecations of this option see the [checkout](https://chatsooner.com) section of this document.Defaults to false.
collectPaymentMethod | optional | Requires an active payment method to be set. See [here](https://chatsooner). If set to true all payment options will be displayed to to the user and they have to succesfully complete the payment for the order to be placed.
collectShippingOptions | optional | Requires active shiping methods to be set. See [here](https://chatsooner.com). Defaults to false
collectEmail | optional | Collects a users email address on checkout.Defaults to false
collectPhysicalAddress | optional | Collects the users physical address on checkout. Defaults to false.

## Edit Product

> Use this to edit a product: 

```shell
curl --request POST \
  --url https://api.chatsooner.com/edit/store/product \
  --header 'Authorization: [API Key]' \
  --header 'Content-Type: multipart/form-data' \
  --form 'productId=Your Product ID'
```

```javascript
//No javascript library available for this endpoint. Use Shell instead
```

**All fields** available when you add a product can be edited. See the *Adding Products* parameter reference to see the required field formating.
### HTTPS Request

`POST https://api.chatsooner.com/edit/store/product`

The <code>productId</code> is the only required field for this endpoint. To alter any other field simply include the <code>field</code> and <code>value </code>pair that needs changing.


<aside class="notice">
The <code>imageUrl</code> field is available to change the main image. Altering the images field does not add new images it replaces the whole array. Use wisely.
</aside>

## Add Product Variants

> Use this to add product variants```Add all required product fields and any other supported fields you wish to add```

```shell
curl --request POST \
  --url https://api.chatsooner.com/edit/store/product \
  --header 'Authorization: [API Key]' \
  --header 'Content-Type: multipart/form-data' \
  --form 'productId=Your Product ID'
  --form variant=true
```

```javascript
//No javascript library available for this endpoint. Use Shell instead
```

This is the **same endpoint used to edit products**. The same requirement for the <code>productId</code> holds. Add a new field <code>variant</code> set to <code>true</code> and this endpoint will add a variant for this product.

### HTTPS Request

`POST https://api.chatsooner.com/edit/store/product`

A variant is a **product**. See the adding [products](#adding-products) reference for the fields required and available for all products. 

<aside class="notice">
The <code>productName</code> field should be the full variant specification. For example <code>Red, Size 7, Running Shoe<code> this is because WhatsApp does not fully cater for variants and this is the current means to achieve variant functionality
</aside>

## Retrieve products

```shell
curl --request GET \
  --url https://api.chatsooner.com/retrieve/store/products \
  --header 'Authorization: [API Key]' \
```

```javascript
//No javascript library available for this endpoint. Use Shell instead
```

> The above command returns JSON structured like this:

```json
[
  {
    "retailerId": "p_yZheZwTbbDtc",
    "description": "The GTX 22 Cart Bag is made with cushioned and air mesh fabric for durability and long-lasting quality. This cart bag includes 2 full length dividers, a 4-way top with an integrated handle and 6 pockets for your storage convenience on the course. This cart bag features a lightweight high mount automatic stand system 
and a moulded angle base design for added stability. This cart bag has a crossbow dual strap for supreme carrying comfort on the course. \r\n' +
      '\r\n' +
      'Features: \r\n' +
      '-6 pockets \r\n' +
      '-2-full length dividers \r\n' +
      '-Extra-large travel and rain hood\r\n' +
      '-Cushioned fabric with air mesh fabric \r\n' +
      '-4-way top with an integrated grab handle \r\n' +
      '-Crossbow dual strap for max balance and comfort\r\n' +
      '-Moulded angle base designed for greater bag stability\r\n' +
      '-Lightweight high mount automatic stand system for added stability'",
    "amount": "1899.99",
    "locations": [],
    "currency": "ZAR",
    "variations": [],
    "productUrl": "https://www.theproshop.co.za/product/6000536-bag-cart-gtx-22",
    "collectPaymentMethod": true,
    "productName": "GTX 22 Cart Bag",
    "collectEmail": true,
    "promptTemplate": "500c1d89-3fa8-4a58-8ea7-b37f555d3557",
    "brand": "GTX",
    "images": [],
    "hasNoCheckout": false,
    "specifications": {
      "Bag Type": "Cart Bag",
      "Club Divider Number": "4 Way",
       "Brand": "GTX",
       "Number of Pockets": "6"
    },
    "aiDescription": "'\n' +
      '\n' +
      'Introducing the GTX 22 Cart Bag, the perfect bag for your golfing needs. This incredible bag features a 4 way club divider and 6 pockets so you can store all your golfing accessories with ease. The GTX 22 Cart Bag is designed to be used on a cart and is the perfect companion for a day out on the golf course. Made with the highest quality materials, this bag is sure to last you for years to come. Get the GTX 22 Cart Bag today and make your next golfing experience the best it can be!",
    "category": "Bags & Carts",
    "merchantId": "BphZYXWlCZVV8cCOMc5M",
    "facebook_product_id": "5796799977082979",
    "tags": [],
    "isActive": true,   
    "soldItems": 0,
    "botId": "ChatsoonerDemo",
    "productId": "p_yZheZwTbbDtc",
    "collectPhysicalAddress": true
  }
]
```

This endpoint retrieves all products.

### HTTPS Request

`GET http://api.chatsooner.com/retrieve/store/products`


<aside class="success">
Some new fields can be seen inside the product
</aside>
