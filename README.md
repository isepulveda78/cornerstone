# Test Description

## Overview
When I started the test, I created a category called Special Items. Then I created a product named Special Item, uploaded two images, and then assigned it to the Special Items category. I went to inspect the product with my devtools to look for specific classes. I found the class 'productView-img-container' and then searched for the class in Visual Studio code to find that I could start working on the product-view.html file. I noticed that that the unordered list had the image that I wanted use to change the main picture when I hovered over it, so I created a variable and grabbed the id from the unordered list. 

variable: 

let productThumbnail = document.getElementById('productView-thumbnails');

After creating the variable, I console.logged it to see what data I was working with. I noticed to get the image that I wanted I had to chain my object down.

The hover image:

productThumbnail.lastElementChild.childNodes[0].nextSibling.dataset.imageGalleryNewImageUrl

The hover out image:

productThumbnail.firstElementChild.childNodes[0].nextSibling.dataset.imageGalleryNewImageUrl

To get it to work, I noticed that I needed to include a secret key.

Secret Keys: 
productThumbnail.lastElementChild.childNodes[0].nextSibling.dataset.imageGalleryNewImageSrcset
productThumbnail.firstElementChild.childNodes[0].nextSibling.dataset.imageGalleryNewImageSrcset

### What the code looks like:
```
$(document).ready(function () {
        let productThumbnail = document.getElementById('productView-thumbnails');
$('.productView-image--default')
    .mouseover(function () {
    $(this).attr("src", productThumbnail.lastElementChild.childNodes[0].nextSibling.dataset.imageGalleryNewImageUrl);
    $(this).attr("srcset", productThumbnail.lastElementChild.childNodes[0].nextSibling.dataset.imageGalleryNewImageSrcset);
})
    .mouseout(function () {
    $(this).attr("src", productThumbnail.firstElementChild.childNodes[0].nextSibling.dataset.imageGalleryNewImageUrl);
    $(this).attr("srcset", productThumbnail.firstElementChild.childNodes[0].nextSibling.dataset.imageGalleryNewImageSrcset);
    });
});
```

## Add All To Cart
In the product-listing.html file, I used the APIs endpoints that I found on the BigCommerce documentation (https://developer.bigcommerce.com/api-docs/storefront/add-to-cart-url). I noticed that the Stencil CLI uses SweetAlerts, so I grabbed the CDN from https://sweetalert2.github.io/.

### Fetching Data Code

```
$("#addToCart").click(function() {
	// add product id 123
    return $.get("/cart.php?action=add&product_id=112")
	.done(function(data, status, xhr) {
		Swal.fire({
        position: 'top-end',
        icon: 'success',
        title: 'You have added some sweet cats to your cart.',
        showConfirmButton: false,
        timer: 1500 })
	})
	.fail(function(xhr, status, error) {
		console.log('oh noes, error with status ' + status + ' and error: ');
		console.error(error);
		return xhr.done();
	})
	.always(function() {
		return window.location = "/special-items";
	});
});
```

## Getting Shopping Cart Data
To get the what's in the cart, I refered to the documentation (https://developer.bigcommerce.com/api-docs/storefront/tutorials/working-sf-apis).

### Fetching the Data from Cart
```
const data = fetch('/api/storefront/carts?include=lineItems.physicalItems.options', {
                method: "GET",
                credentials: "same-origin"
                })
                .then(response => {
                    return response.json()
                })
            .catch(error => console.error(error));
```

### Getting Cart ID & Showing 'Remove All Items' Button

```
const getItems = async () => {
    let result = await data
    let cartId = result[0].id
    if(cartId){
        $('#removeFromCart').css('display', 'inline-block')
    }
}
getItems()
```

## Deleting All Items in the Shopping Cart
I refered to the documentation (https://developer.bigcommerce.com/api-docs/storefront/tutorials/working-sf-apis).

### Deleting ALL Items from Cart Code
```
const deleteItem = async () => {
    const result = await data
    const cartId = result[0].id
    console.log(cartId)
    return fetch(`/api/storefront/carts/` + cartId, {
      method: "DELETE",
      credentials: "same-origin",
      headers: {
        "Content-Type": "application/json",
      }
    })
    .then(response => {
        return response
    })
    .catch(error => console.error(error))
}
```

### After Clicking on the 'Remove ALL Items' Button & Notifying the Customet that Cart is Empty
```
$("#removeFromCart").click(function() {
    $('#removeFromCart').css('display', 'none')
    deleteItem()
    Swal.fire({
        position: 'top-end',
        icon: 'success',
        title: 'Your cart is empty.',
        showConfirmButton: false,
        timer: 1500
    })
    window.location = "/special-items";
});
```

## Adding a Banner
When I created the banner, I inspected the website with my devtools. I then looked for the header.html file to add my banner. I referred to https://developer.bigcommerce.com/theme-objects/schemas#customer for help.

### Banner Code
```
{{#if customer}}
<div class="banners">
    <div class="banner__content">
    <div class="banner__text">
        {{customer.name}} <br />
        {{customer.email}} <br />
        {{customer.shipping_address.address1}}
    </div>
    </div>
</div>
{{/if}}
```

### Site: https://special-items.mybigcommerce.com/
### Preview Code: uhi4m0oe4l
