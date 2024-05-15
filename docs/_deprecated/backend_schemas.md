client (user with client role, or without role but that who authorized in app)
client-profile
    contact phone (default to authorization phone)
    name
    surname
    avatar
    

client_address:
    id: uuid
    client_id: Ref<client>
    title: text - what user is input
    lat: double
    lng: double 

city - main purpose to organize restaurant by geo affinity
    id: uuid
    title: string
    address: Ref<address>
    lat: double
    lng: double

restaurant 
    id: uuid
    city_id: Ref<city>
    owner_id: Ref<client>
    updated_at: timestamp
    title: string #localization is required?
    description: string
    address: Ref<address>

product
    id: uuid
    restaurant_id: Ref<restaurant>
    price: decimal # different currincies?
    status - draft (hiden, can edit), active (show to client, can't edit), archived (accesed only from history)
    updated_at: timestamp
    #translation: Rel<product_translation>

product_translation:
    id: uuid
    product_id: Ref<product>
    locale: Ref<locale>
    title: string
    description: string
    image: Ref<image>

order
    id: uuid
    order_status - initial, prepare, delivery, complete, canceled_by_client, canceled_by_restaurant
    payment_status - none, complete
    user_comment: text
    address: Ref<client_address>

oreder_item
    order_id: Ref<order>
    product_id: Ref<product.archive>



product.archive (product_id,product_updated_at) 
    - contains specific version of product in moment when order is created, order refer to this as product_id
     allow to see order in moment of it's creation

product_translation.archive:
    

image
    id: uuid
    image_file: Ref<Files>
    blurhash: string
    width: int
    height: int

notification:
    id: uuid
    type: enum (custom (create by admin, manager) | (service - order status change etc))
    title: string
    target: audience | client


client interactions
    authorize(register) - in otp flow is the same
    <!-- PRODUCTS -->
    view products
    search products
    view product detail
    <!-- RESTAURANT -->
    view restaurants
    view restaurant detail
    <!-- CART -->
    manage cart
        add products
        remove products
    <!-- ORDERS -->
    create order
    cancel order
    view previous orders
    view order detail
    repeat orders
    <!-- # REVIEWS -->
    <!-- # SUPPORT/ORDER CHAT -->

restaraunt manager interactions (need to define access rules)
    <!-- RESTAURANTS -->
    create restaurant
    view own restaurants
    edit own restaraunt
    <!-- PRODUCTS -->
    create product
    view own products
    edit own product
    <!-- ORDERS -->
    view own.restaraunt orders
    view order creator info
    change own.restaraunt order status to [cancel_by_restaraunt, prepare]
    <!-- # REVIEWS -->
    <!-- # SUPPORT/ORDER CHAT -->

delivery
    <!-- it's part of the system -->
    take order into delivery
    change current_delivery.payment_status status to [complete]
    change current_delivery.order status to [complete]
    view current_delivery.order info
    view current_delivery.order.creator info