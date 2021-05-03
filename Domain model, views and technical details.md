### [See the application prototype!](https://www.figma.com/proto/bmeV3VMz8Aool0cum8YaqX/KPABD-e-store?node-id=118%3A808&scaling=min-zoom&page-id=28%3A9)

## Views-operations relations
Here I define which actions/operations are preformed by which views.


| View | Operation | Information entities |
| -------- | -------- | -------- |
| sign up page | create account | user |
| sign in page | login | user |
| forget password | modifying an account | user |
| main page | browsing | product, feed |
| main page | browsing | feed, product, product category |
| categories | browsing | product category |
| catalog (i.e. product list) | browsing, sorting, filtering | product |
| product card (i.e. product page) | viewing product | product |
| rating and reviews | reading an opinion/rating | product, opinion |
| rating and reviews add modify | adding/modifying an opinion/rating | product, opinion |
| my bag (i.e. cart) | browsing, placing an order, applying a discount | product, user |
| checkout | placing an order, choosing shipping address/billing address, choosing a delivery type | product, user, order, address, shipping option |
| my profile | viewing/modifying an account | user, order |
| my orders | viewing orders | order |
| order details | viewing/modifying order | order |
| profile settings | viewing/modifying an account settings | user |
| shipping addresses | viewing shipping addresses | address |
| add shipping addresses | adding shipping addresses | address |
| modify shipping addresses | modifying shipping addresses | address |

## Drafts of information entities

### User
#### Properties:
* full_name: `type=string, max_size=50, required=True`
* date_of_birth: `type=date`
* email: `type=email/string with regex, required=True, max_size=50`
* billing_address: `required=True, type=ObjectId` - id of `Address`
* shipping_addresses: `List[ObjectId], required=True`
* password: `type=string/password, required=True, max_size=50`
* last_updated: `type=date, required=True`
* created_at: `type=date, required=True`
#### Methods:
* on_update - this is a function/decorator that changes the `last_updated` field of User when user is modified
* change password
* reset_password
* add_shipping_address
* remove_shipping_address

---

### Feed
#### Properties:
* date: `type=date, required=True`
* title: `type=string, max_size=50, required=True`
* description: `type=string, max_size=500, required=True`
* photo: `type=string, max_size=200` - it's a url to a server resource
* url: `type=string, max_size=200` - it's a action, url to page of what this feed refers to, for example some sale page
* end_date: `type=date, required=True`
#### Methods:
No methods, only base CRUD functionalities will be used.

---

### ProductCategory
#### Properties:
* name: `type=string, max_size=50`
* subcategory: `type=ObjectId`
### Methods:
No methods, only base CRUD functionalities will be used.

---

### Product
#### Properties:
* name: `type=string, max_size=50, required=True`
* brand: `type=string, max_size=50, required=True`
* desciption: `type=string, max_size=500, required=True`
* category: `type: ObjectId, required=True`
* photos: `type=List[type=string, max_size: 200], required=True` - list of the product images' urls
* added: `type=date, required=True`
* properties: `type=object, required=True` - it's a indented object (MongoDB):
    * sizes: `type=List[string | decimal]`
    * colors: `type=List[type=string, max_size=40]`
* price: `type: decimal, required=True`
* discount: `type: decimal`
* rating: `type: ObjectId` - id of Rating object
* opinions: `List[ObjectId]` - ids of Opinions objects
* delivery_options: `List[ObjectId], required=True, nonempty=True` - ids of DeliveryOptions objects
#### Methods:
* add_photos
* add_photo
* remove_photo
* add_property
* modify_property
* remove_property
* calculate_price_after_discount
* rate
* add_delivery_option
* remove_delivery_option
* add_opinion
* remove_opinion

---

### Order
#### Properties:
* user: `type=ObjectId, required=True`
* products: `List[object], required=True` where `object`:
    * product_id: `type: ObjectId`
    * properties: `type=object`:
        * size: `type=string | decimal`
        * color: `type=string, max_size=40`
* cost: `type=decimal, required=True`
* total_discount: `type=decimal`
* status: `type=string, required=True` - one of:
    * "Awaiting confirmation"
    * "Preparing for shipping"
    * "Shipped"
    * "Delivered"
* shipping_address: `type: ObjectId, required=True` - id of the shipping address object
#### Methods:
No methods, only base CRUD functionalities will be used.

---

### Rating
#### Properties:
* stars: `type=int,required=True`
* user: `type=ObjectId, required=True`
* date: `type: date, required=True`
#### Methods
No methods, only base CRUD functionalities will be used.

---

### Opinion
#### Properties:
* user: `type=ObjectId, required=True`
* date: `type: date, required=True`
* photos: `List[type=string, max_size=200]` - urls to a server resource
* content: `type=string, max_size=500, required=True`
#### Methods
No methods, only base CRUD functionalities will be used.

---

### Address
#### Properties:
* title: `type=string, max_size=50, required=True`
* street: `type=string, max_size=100, required=True`
* house_number: `type=string, max_size=10, required=True`
* apartment_number: `type=int`
* postal_code: `type=string, max_size=6, required=True`
* City: `type=string, max_size=100, required=True`
### Methods
No methods, only base CRUD functionalities will be used.

---

### ShippingOption
#### Properties:
* name: `type=string, max_size=50, required=True`
* price: `type=decimal, required=True`
### Methods
No methods, only base CRUD functionalities will be used.


## Draft of the domain model

### Entities:
* User
* Product
* Feed
* Order

### Value objects:
* ProductCategory
* Address
* Rating
* Opinion
* ShippingOption

---

### Lifecycles:
* User:
    1. Created
    2. Active
    3. Unactive
    4. Removed
* Product:
    1. Added
    2. Available
    3. Unavailable:
        4. Available - added more porducts (go back to stage 2)
* Feed
    1. Added
    2. Available
    3. Unavailable
* Order:
    1. Awaiting confirmation
    2. Preparing for shipping
    3. Shipped
    4. Delivered

### Aggregates:
* Product:
    * Product - root
    * Rating
    * Opinion
* User:
    * root - User
    * Address

![](https://i.imgur.com/A60vNrS.jpg)
