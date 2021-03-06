Make sure you execute the commands above 👆

**IMPORTANT** 👇

Copy the code from yesterday:

```bash
cp -r ../../05-Food-Delivery-Day-One/01-Food-Delivery/{app,data,app.rb,router.rb} . # trailing dot is important
```

Then, before you start, check that it still works:

```bash
rake
```

Now continue with your own code and keep adding features to the router / making the `rake` greener. Of course, you can have a look at [this morning's code](https://github.com/lewagon/food-delivery/tree/lecture-day-two) to get some inspiration.

Let's add more features to our Food Delivery program!

Here are all the **user actions** of our app:
[ ] As an employee, I can log in
[X] As a manager, I can add a new meal
[X] As a manager, I can list all the meals
[X] As a manager, I can add a new customer
[X] As a manager, I can list all the customers
[ ] As a manager, I can add a new order
[ ] As a manager, I can list all the undelivered orders
[ ] As a delivery guy, I can mark one of my orders as delivered
[ ] As a delivery guy, I list all my undelivered orders

Hence,there are two new components:
- **Employees**
- **Orders**

## 1 - `Employee`

### 1.1 - `Employee` model

Our restaurant has two types of employees, **managers** and **delivery guys**. Both have an id, a username and a password, but they have different privileges depending of their roles.

Write some code to implement this and crash-test your model in `irb`. Then test your code by running `rake employee`.

All green? Good! Time to `git add`, `commit` and `push`.

### 1.2 - `Employee` repository

Now that we have a model representing our employees, we need a repository to store them.

This repository is initialized with a CSV file path. It has a **read-only** logic since only the administrator of our app can create accounts (no need for and `add` method). The interface of this repository allows to:
- Get all the delivery guys from the repository
- Find a specific employee thanks to its id
- Find a specific employee thanks to username

Write some code to implement this and crash-test your repository in irb. You should create your own `employees.csv` CSV file inside the `data` folder. Then test your code by running `rake employee`.

All green? Good! Time to `git add`, `commit` and `push`.

### 1.3 - `Session` controller

Let's implement a **login** logic in our app.

We want to have two menus in the router: one listing the tasks for managers and one listing the tasks for delivery guys. We also want to route the employee's choice to the corresponding action of the matching controller.

To handle that, we'll introduce the notion of a **session**. At the router level, we'll store the logged-in user in a session.

The sign-in sequence should go like this:

```bash
> username?
paul
> password?
blablabla
Wrong credentials... Try again!
> username?
paul
> password?
secret
Welcome Paul!
```

After signing in, the dashboard that you see should be **dependent on your role**.

Write some code to implement this behavior.

There is no rake for this part. Launch your app by running this command in the terminal:

```bash
ruby app.rb
```

Everything is working? Good! Time to `git add`, `commit` and `push`.

## 2 - `Order`

### 2.1 - `Order` model

Our restaurant takes orders, so we need a way to represent what an order is.


An order is what ties everything together. Each order has an id, a meal, a customer, an employee plus a `delivered` boolean to record whether or not the order has been delivered.

Write some code to implement this and crash-test your model in `irb`. Then test your code by running `rake order`.

All green? Good! Time to `git add`, `commit` and `push`.

### 2.2 - `Order` repository

Now that we have a model representing our orders, we need a repository to store them.

This repository is initialized with a CSV file path. It read/write the orders from the CSV file and store them in memory. The interface of this repository allows to:
- Add a new order to the repository
- Get all the undelivered orders from the repository

Since an order has a `meal`, a `customer` and an `employee` **instances**, we also need to initialize our order repository with a meal repository, a customer repository and an employee repository.

Write some code to implement this and crash-test your repository in irb. You should create your own `orders.csv` CSV file inside the `data` folder. Then test your code by running `rake order`.

**Important**: the `order_repository` tests run by `rake` **only work if you define the parameters in `#initialize` in the same order as in the tests**:

```ruby
class OrderRepository
  def initialize(orders_csv_path, meal_repository, customer_repository, employee_repository)
    # [...]
  end

  # [...]
end
```

All green? Good! Time to `git add`, `commit` and `push`.

### 2.3 - `Order` controller

Let's move to the controller. Here are the **user actions** we want to implement:
- As a manager, I can add a new order
- As a manager, I can list all the undelivered orders
- As a delivery guy, I can mark one of my orders as delivered
- As a delivery guy, I list all my undelivered orders

Remember that the role of the controller is to delegate the work to the other components of our app (model, repository and views)!

Start by writing the **pseudocode**, breaking each user action into elementary steps and delegating each step to a component (model, repository or views). Then write the code to implement each step. Create the view and code it step by step.

To test your controller, link it to your app by instanciating it in `app.rb` and passing it to the router. Then you can crash-test your code by launching your app:

```bash
ruby app.rb
```

`rake order` should also help you go through all these steps. Follow your guide!

Make sure your four order user actions work before moving on to the next feature.

**Important**: the `orders_controller` tests run by `rake` **only work if you define the parameters in `#initialize` in the same order as in the tests**:

```ruby
class OrdersController
  def initialize(meal_repository, customer_repository, employee_repository, order_repository)
    # [...]
  end

  # [...]
end
```

All green? Good! Time to `git add`, `commit` and `push`.

## 3 - Optionals

### 3.1 - Implement update and destroy actions for meal and customer

In our app, a manager can't update or destroy an existing order.

Implement these additional user actions:
- As a manager, I can update an existing order
- As a manager, I can delete an existing order

Done? Time to `git add`, `commit` and `push`.

### 3.2 - Hide the user's password

At the moment, a user's password is stored straight in the CSV and is visible to anyone. Is that a good idea? What could we do instead?

Done? Time to `git add`, `commit` and `push`.

You're done with Food Delivery!
