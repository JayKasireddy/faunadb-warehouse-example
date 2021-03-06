# Warehouse Example
## Introduction

![alt text](images/warehouse.png "Warehouse")

__Table of Contents__

* [Setup Schema](#setup-schema)
* [Operations](#operations)
  * [Warehouses](#warehouses)
    * [Create Warehouse](#create-warehouse) 
    * [Read Warehouse](#read-warehouse) 
    * [Read All Warehouses](#read-all-warehouses)
    * [Update Warehouse](#update-warehouse)
    * [Replace Warehouse](#replace-warehouse)
    * [Delete Warehouse](#delete-warehouse)
  * [Products](#products)
    * [Create Product](#create-product) 
    * [Read Product](#read-product)
    * [Read Product History](#read-product-history)
    * [Read All Products](#read-all-products)
    * [Update Product](#update-product)
    * [Replace Product](#replace-product)
    * [Delete Product](#delete-product)
  * [Customers](#customers)
    * [Create Customer](#create-customer) 
    * [Read Customer](#read-customer) 
    * [Read All Customers](#read-all-customers)
    * [Update Customer](#update-customer)
    * [Replace Customer](#replace-customer)
    * [Delete Customer](#delete-customer)
  * [Orders](#orders)
    * [Submit Order](#submit-order)

## Setup Schema

### Create Classes
```
CreateClass({name: "warehouses"});
CreateClass({name: "products"});
CreateClass({name: "customers"});
CreateClass({name: "orders"});
```

### Create Indexes
```
CreateIndex(
  {
    "name": "all_warehouses",
    "source": Class("warehouses")
   }
);

CreateIndex(
  {
    "name": "all_products",
    "source": Class("products")
   }
);

CreateIndex(
  {
    "name": "all_customers",
    "source": Class("customers")
   }
);

CreateIndex(
  {
    "name": "all_orders",
    "source": Class("orders")
   }
);

```

## Operations

### Warehouses

#### Create Warehouse

It creates a new Warehouse instance.

```
Create(
  Class("warehouses"), { 
    data: { 
      "name": "East", 
      "address": { 
        "street": "13 Pierstorff Drive", 
        "city": "Washington",
        "state": "DC",
        "zipcode": "20220"
      }
    }
  }
);
```

#### Read Warehouse

It retrieves a Warehouse by its Id.

```
Get(Ref(Class("warehouses"), "1549398205020000"));
```

#### Read All Warehouses

It reads all existent Warehouses.

```
Map(
  Paginate(Match(Index("all_warehouses"))),
  Lambda("nextRef", Get(Var("nextRef")))
);
```

#### Update Warehouse

It partially updates a Warehouses with the given data.

```
Update(
  Ref(Class("warehouses"), "1549398205020000"), {
    data: {
      "address": { 
        "street": "1 Corry Plaza", 
        "city": "Stockton",
        "state": "CA",
        "zipcode": "95205" 
      }
    }
  }
);
```

> NOTE: The `update` function updates specific fields on an instance. It preserves the old fields if they are not specified in `params`. In the case of objects, the old and the new values are merged. If `null` is specified as a value for a field, it is removed.

#### Replace Warehouse

It replaces an existent Warehouse instance with the given data.

```
Replace(
  Ref(Class("warehouses"), "1549398205020000"), {
    data: { 
      "name": "East", 
      "address": { 
        "street": "1 Corry Plaza", 
        "city": "Stockton",
        "state": "CA",
        "zipcode": "95205" 
      }
    }
  }
);
```

> NOTE: The `replace` function, replaces the instance data with the fields provided in `params`. Therefore, old fields not mentioned in `params` are removed.

#### Delete Warehouse
It deletes a Warehouse for the given Id.

```
Delete(Ref(Class("warehouses"), "1549398205020000"))
```

### Products

#### Create Product
It creates a new Product instance.

```
Create(
  Class("products"), {
    data: {
      "name": "Cup",
      "description": "Translucent 9 Oz",
      "price": "6.90",
      "quantity": 105,
      "warehouseId": Ref(Class("warehouses"), "1549398205020000"),
      "backorderLimit": 10,
      "backordered": false
    }
  }
);
```

#### Read Product

It retrieves a Customer by its Id.

```
Get(Ref(Class("products"), "220510522473185799"));
```

### Read Product History
It reads Product's transaction history over time.

```
Paginate(Ref(Class("products"), "220510522473185799"), { "events": true })
```

#### Read All Products

It reads all existent Products.

```
Map(
  Paginate(Match(Index("all_products"))),
  Lambda("nextRef", Get(Var("nextRef")))
);
```

#### Update Product

It partially updates a Product with the given data.

```
Update(
  Ref(Class("products"), "220510522473185799"), {
    data: {
      "price": "7.90"
    }
  }
);
```

> NOTE: The `update` function updates specific fields on an instance. It preserves the old fields if they are not specified in `params`. In the case of objects, the old and the new values are merged. If `null` is specified as a value for a field, it is removed.

#### Replace Product

It replaces an existent Product instance with the given data.

```
Replace(
  Ref(Class("products"), "220510522473185799"), {
    data: {
      "name": "Cup",
      "description": "Translucent 9 Oz",
      "price": "7.90",
      "quantity": 105,
      "warehouseId": Ref(Class("warehouses"), "1549398205020000"),
      "backorderLimit": 10,
      "backordered": false
    }
  }
);
```

> NOTE: The `replace` function, replaces the instance data with the fields provided in `params`. Therefore, old fields not mentioned in `params` are removed.

#### Delete Product
It deletes a Product for the given Id.

```
Delete(Ref(Class("products"), "220510522473185799"))
```

### Customers

#### Create Customer

It creates a new Customer instance.

```
Create(
  Class("customers"), { 
    data: { 
      "firstName": "Auria", 
      "lastName": "Osgardby", 
      "address": { 
        "street": "87856 Mendota Court", 
        "city": "Idaho Falls",
        "state": "ID",
        "zipcode": "83405" 
      }, 
      "telephone": "208-346-0715",
      "creditCard": {
        "network": "Visa",
        "number": "4916112310613672"
      }
    }
  }
);
```

#### Read Customer

It retrieves a Customer by its Id.

```
Get(Ref(Class("customers"), "1520225686617873"));
```

#### Read All Customers

It reads all existent Customers.

```
Map(
  Paginate(Match(Index("all_customers"))),
  Lambda("nextRef", Get(Var("nextRef")))
);
```

#### Update Customer

It partially updates a Customer with the given data.

```
Update(
  Ref(Class("customers"), "1520225686617873"), {
    data: {
      "telephone": "719-872-8799"
    }
  }
);
```

> NOTE: The `update` function updates specific fields on an instance. It preserves the old fields if they are not specified in `params`. In the case of objects, the old and the new values are merged. If `null` is specified as a value for a field, it is removed.

#### Replace Customer

It replaces an existent Customer instance with the given data.

```
Replace(
  Ref(Class("customers"), "1520225686617873"), {
    data: {
      "firstName": "Auria",
      "lastName": "Osgardby",
      "address": {
        "street": "87856 Mendota Court",
        "city": "Idaho Falls",
        "state": "ID",
        "zipcode": "83405"
      }, 
      "telephone": "719-872-8799",
      "creditCard": {
        "network": "Visa",
        "number": "4916112310613672"
      }
    }
  }
);
```

> NOTE: The `replace` function, replaces the instance data with the fields provided in `params`. Therefore, old fields not mentioned in `params` are removed.

#### Delete Customer
It deletes a Customer for the given Id.

```
Delete(Ref(Class("customers"), "1520225686617873"))
```

### Orders

#### Submit Order

In order to submit an Order create below `User-Defined Function`. When called, it will validate the products stock quantity, update it accordingly, check their backorder limit and create a new Order with all of the information.

```
CreateFunction(
  {
    "name": "submit_order",
    "body": Query(
      Lambda(["customerId", "products"],
        // 1- Get Cusomter and Products
        Let(
          {
            "customer": Get(Ref(Class("customers"), Var("customerId"))),
            "products": 
              Map(
                Var("products"),
                Lambda("requestedProduct",
                  Let(
                    {
                      "product": Get(Ref(Class("products"), Select("productId", Var("requestedProduct"))))
                    },
                    {
                      "ref": Select("ref", Var("product")),
                      "price": Select(["data", "price"], Var("product")),
                      "currentQuantity": Select(["data", "quantity"], Var("product")),
                      "requestedQuantity": Select(["requestedQuantity"], Var("requestedProduct")),
                      "backorderLimit": Select(["data", "backorderLimit"], Var("product"))
                    }
                  )
                )
              )
          },
          Do(
            // 2- Check if there's enough stock
            Foreach(Var("products"),
              Lambda("product",
                If(
                  LTE(Select("requestedQuantity", Var("product")), Select("currentQuantity", Var("product"))),
                    Var("product"),
                    Abort(Concat(["Stock quantity for Product [", Select(["ref", "id"], Var("product")), "] not enough – requested at [", ToString(Time("now")), "]"]))
                )
              )
            ),
            // 3- Update products stock
            Foreach(Var("products"),
              Lambda("product",
                Update(
                  Select("ref", Var("product")), {
                    data: {
                      "quantity": Subtract(Select("currentQuantity", Var("product")), Select("requestedQuantity", Var("product")))
                    }
                  }
                )
              )
            ),
            // 4- Update backordered status
            Foreach(Var("products"),
              Lambda("product",
                If(
                  LTE(Subtract(Select("currentQuantity", Var("product")), Select("requestedQuantity", Var("product"))), Select("backorderLimit", Var("product"))),
                  Update(
                    Select("ref", Var("product")), {
                      data: {
                        "backordered": true
                      }
                    }
                  ),
                  Var("product")
                )
              )
            ),
            // 5- Create Order
            Let(
              {
                "orderProducts":
                  Map(
                    Var("products"),
                    Lambda("product", 
                      {
                        "productId": Select("ref", Var("product")),
                        "quantity": Select("requestedQuantity", Var("product")),
                        "price": Select("price", Var("product"))
                      }
                    )
                  )
              },
              Create(
                Class("orders"), {
                  data: {
                    "customerId": Select("ref", Var("customer")),
                    "line": Var("orderProducts"),
                    "status": "processing",
                    "creationDate": ToString(Time("now")),
                    "shipDate": null,
                    "shipAddress": Select(["data", "address"], Var("customer")),
                    "creditCard": Select(["data", "creditCard"], Var("customer"))
                  }
                }
              )
            )
          )
        )
      )
    )
  }
);
```

In order to call the function, execute below command: 

```
Call(
  Function("submit_order"), 
    "1",
    [
      Object({
        "productId": "1",
        "requestedQuantity": 12
      }),
      Object({
        "productId": "2",
        "requestedQuantity": 22
      }),
      Object({
        "productId": "3",
        "requestedQuantity": 8
      })
    ]
);
```
