# KGuzikowski-KPABD-e-shop-api

## Design and architecture:

### Main high-level business process which includes key activities.
The main purpose of ths project is to create a fully functioning but limited in functionalities online store api (there will be no integration with payment systems).

Through that api, front-end app will be able not only to browse all of the currently available products but also the unavailable, historical, products' data.

---

#### Let's focus on the most important thing - browsing products.
User will go to products page and will see all products, or a result of some different query - filtered list.
Products are divided into categories and each category might be divided into subcategories and each subcategory might be divided into subcategories, and so on.

User will be able go choose a category that suits them and will be presented with a list of all the products belonging to the chosen category.
When I say that user will be presented with a list of all the products belonging to the chosen category, I don't mean that all of the products will be shown at once. Pagination will be implemented so that on each page user will only see some specific number of products. That number can also be specified by the user.

#### Unavailable products.
As I already mentioned, the api will allow to get the historical data. Such data can and will be used for reporting and statistical purposes.

*Note, by simply using for example sklearn we can already get some powerful machine learning reasoning that will help maximize the revenue.*

---

Here it's worth mentioning that since the vast majority of actions taken on online stores are browsing, it's important to optimize such functionality. That can be simply achieved by having two collections consisting of:
1) available products,
2) unavailable products.

How do I know that the vast majority of actions taken on online stores are browsing?
By analyzing web traffic on the existing online store and reading statistics I was able to find the internet.

---

#### Buying and delivery
User, of course, will be able to buy products.

By adding products to the cart, the app will gather information which products should be sold.
User can buy many products at once. For now the limit is 1000. When bought, all of the acquired products will become a one order.

To place a order, a user must be logged in.

Placing an order will consist of several steps, excluding payment, because integration with external payment systems is out of the scope of this university course.

Those steps are:
1) Filling the form with user data.
2) Choosing on of the existing shipping addresses or adding a new one.
3) Choosing delivery method.
4) Confirming the order.

So the api will get a rather big request consisting of information mentioned above.

Each order has a status assigned to it. Possibilities are:
* awaiting confirmation - this status is assigned until payment is conformed - so in this project this status will never be assigned
* preparation for shipping
* shipped
* delivered

Once the order is placed, the delivery method cannot be changed. But before status is changed to "shipped" user can change the user data and the shipping address.

---

Those are the base functionalities that most standard online stores have to implement.

---

### A list of the key business entities
Main entities with some of the most important properties are defined below.

1. User:
    * First name
    * Last name
    * Email
    * Billing address as a `Address`
    * Shipment addresses as a `List[Address]`
2. Product:
    * Name
    * Brand
    * Description
    * Photos (as a list of urls)
    * Price
    * Available sizes
    * Rating
    * Opinions as a `List[Opinion]`
    * Delivery options as a `List[DeliveryOption]`
3. Order:
    * Products as a `List[Product]`
    * Cost
    * Status
    * User as a `User`
    * ShippingAddress as a `Address`
4. Address - specification not needed for now
5. Opinion:
    * Date
    * User as a `User`
    * Content
    * Product as a `Product`
6. DeliveryOption - not yet defined

### All key functionalities
* browsing
* searching
* filtering
* choosing a delivery type
* changing or adding a shipping address/billing address
* changing a user data
* adding a product
* removing a product
* modifying a product
* changing a order status
* placing a order
* adding a opinion
* modifying a opinion