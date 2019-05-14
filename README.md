# SQL Relationship Tables By Query Builder and Eloquent ORM
SQL Relationship Tables By PHP Laravel Query Builder and PHP Laravel Eloquent ORM.

## Content:
- [The Steps To Create The Right Relationships Between Tables](#The-Steps-To-Create-The-Right-Relationships-Between-Tables)
- [Relationship Tables types.](#Relationship-Tables-types)
- [To choose The Correct Type of Relationships Between Tables With Examples.](#To-choose-The-Correct-Type-of-Relationships-Between-Tables-With-Examples)
- [Tables Design.](#Tables-Design)
	- [One To One Tables Design](#One-To-One-Tables-Design)
	- [One To many Tables Design](#One-To-many-Tables-Design)
	- [Many to Many Tables Design](#Many-to-Many-Tables-Design)
- [SQL Query Types of relationship.](#SQL-Query-Types-of-relationship)
	- [Inner join](#Inner-join)
	- [Left Join](#Left-Join)
	- [Right join](#Right-join)
- [By SQL Query.](#Relation-By-SQL-Query)
	- [One to One By SQL Query](#One-to-One-By-SQL-Query)
	- [One to Many By SQL Query](#One-to-Many-By-SQL-Query)
	- [Many to Many By SQL Query](#Many-to-Many-By-SQL-Query)
- [By PHP Laravel Query Builder.](#By-PHP-Laravel-Query-Builder)
	- [One to One By Query Builder](#One-to-One-By-Query-Builder)
	- [One to Many By Query Builder](#One-to-Many-By-Query-Builder)
	- [Many to Many By Query Builder](#Many-to-Many-By-Query-Builder)
- [By PHP Laravel Eloquent ORM.](#By-PHP-Laravel-Eloquent-ORM)
	- [One to One By Eloquent](#One-to-One-By-Eloquent)
	- [Inverse One To One By Eloquent](#Inverse-One-To-One-By-Eloquent)
	- [One to One Create Relation and Update Relation and Remove Relation](#One-to-One-Create-Relation-and-Update-Relation-and-Remove-Relation)
	- [One to Many By Eloquent](#One-to-Many-By-Eloquent)
	- [Inverse One To Many By Eloquent](#Inverse-One-To-Many-By-Eloquent)
	- [One to Many Create Relation and Update Relation and Remove Relation](#One-to-Many-Create-Relation-and-Update-Relation-and-Remove-Relation)
	- [Many To Many By Eloquent](#Many-To-Many-By-Eloquent)
	- [Many to Many Create and Update and Remove Relation](#Many-to-Many-Create-and-Update-and-Remove-Relation)

=======================================================
## The Steps To Create The Right Relationships Between Tables:
- Step one: Choose The Correct Type of Relationships Between Tables.
- Step Two: Design Tables.
- Step Three: Write The Correct Query.

=======================================================
## Relationship Tables types:
- One To One. (not Common)
- One to Many. (is very common)
- Many to Many. (a little  common)

========================================================
## To choose The Correct Type of Relationships Between Tables With Examples:

==> **The unite to chose the relationship is row or rows not table.**

### One To One:

           A - user and info
                   - a user has one info == 'One to One'
                   - (Inverse) a info belongs to a user == 'One to One'

           B - order and Bill
                   - a order has one bill == 'One to One'
                   - (Inverse) a bill belongs to a order == 'One to One'



### One To Many:

            A - category and articles
                   - a category has many articles == 'One to Many'
                   - (Inverse) articles belongs to a category == 'Many to one'

            B - Country and states
                   - a country has many states == 'One to Many'
                   - (Inverse) states belongs to a country == 'Many to one'

            C - user and orders
                   - a user has many orders == 'One to Many'
                   - (Inverse) orders belongs to a user == 'Many to one'

            D - author and posts
                   - an author has many posts == 'One to Many'
                   - (Inverse) posts belongs to an author == 'Many to one'

            F - doctor and patients
                   - a doctor has many patients == 'One to Many'
                   - (Inverse) patients belongs to a doctor == 'Many to one'

            G - class and students
                  - a class has many students == 'One to Many'
                  - (Inverse) students belongs to a class == 'Many to one'

            H - user and bills
                  - a user has many bills == 'One to Many'
                  - (Inverse) bills belongs to a user == 'Many to one'

            K - user and bills
                  - a section has many Employees == 'One to Many'
                  - (Inverse) Employees belongs to a section == 'Many to one'




### Many to Many:

               A - articles and tags
                    - a tag has many articles == 'Many to Many'
                    - an article belongs to many tags == 'Many to Many'

               B - authors and posts
                    - an author has many posts == 'Many to Many'
                    - a post belongs to many authors == 'Many to Many'

               C - orders and products
                    - an order has many products == 'Many to Many'
                    - a product belongs to many orders == 'Many to Many'

               D - users and products
                    - a user has many products == 'Many to Many'
                    - a product belongs to many users == 'Many to Many'

               F - classes and students
                    - a class has many students == 'Many to Many'
                    - a student belongs to many classes == 'Many to Many'


========================================================

## Tables Design:

         main table (parent) in relation with subTable (child)

                    relation
            user ---------------> posts
               |                   |
             main Table         subTable
              (parent)           (child)


### One To One Tables Design:

*a sub table has a -> **'unique'** <- column, and the content of this column should has the 'id' of main table.*

**Example:**

            "user_id"->unique

**Migration Example:**


```php
$table->unsignedBigInteger('user_id')->unique();
$table->foreign('user_id')->references('id')->on('users')->onDelete('cascade'); // can add ->nullable()
```

### One To many Tables Design:
*a sub table has a column, and the content of this column should has the 'id' of main table.*

**Example:**

            "user_id"

**Migration Example:**
```php
$table->unsignedBigInteger('user_id');
$table->foreign('user_id')->references('id')->on('users')->onDelete('cascade');
```
### Many to Many Tables Design:
*create new table has two -> **'primary'** <- columns and the content of these columns should have the 'id' of the two main tables.*

**Example:**
```php
$table->unsignedBigInteger('product_id');
$table->foreign('product_id')->references('id')->on('products')->onDelete('cascade');
$table->unsignedBigInteger('order_id');
$table->foreign('order_id')->references('id')->on('orders')->onDelete('cascade');
$table->primary(['product_id', 'order_id']);
        // we can add timestamps if need it.
$table->timestamps();
```
========================================================
## SQL Query Types of relationship:

==> **The Main Idea is show two rows from two tables as a one row by one query**


### There are three methods to do that:-
- Inner join.
- lift join.
- right join.

### Inner join:
**To show row(s) that in relation between main table and sub table in one row.**

 *(see photo: "Inner Join.gif")*

![Inner Join](https://www.w3schools.com/sql/img_innerjoin.gif)

#### -> Basic Query SQL step for Inner join:

```sql
                 SELECT column_name(s)
                 FROM table1
                 INNER JOIN table2
                 ON table1.column_name = table2.column_name;
```




### Left Join:
**To show all rows in left table (table1) and show row(s) that in relation between main table and sub table or (table1) and (table2).**

*(see photo: "left Join.gif")*

![ Left Join](https://www.w3schools.com/sql/img_leftjoin.gif)


#### -> Basic Query SQL step for  Left Join:
```sql
                 SELECT column_name(s)
                 FROM table1
                 LEFT JOIN table2
                 ON table1.column_name = table2.column_name;
```



### Right join:
**To show all rows in right table (table2) and show row(s) that in relation between main table and sub table or (table1) and (table2).**

*(see photo: "Right Join.gif")*

![ Left Join](https://www.w3schools.com/sql/img_rightjoin.gif)
#### -> Basic Query SQL step for Right join:
```sql
                  SELECT column_name(s)
                  FROM table1
                  Right JOIN table2
                  ON table1.column_name = table2.column_name;
```


========================================================
## Relation By SQL Query:

### One to One By SQL Query:

#### -=>The Example in Table Design:
```php
              =>table: users('id', 'name')
              =>table: profiles('id', 'name', 'gender', 'user_id')
```

**->To show all rows:**
```sql
           SELECT users.name, profiles.profile_name as Pro_name, profiles.gender
           FROM users
           INNER JOIN profiles
           ON users.id = profiles.user_id
```

**->To show all rows from main table or left table and if there matching relation from sub table (left Join):**
```sql
           SELECT users.id, users.name, profiles.profile_name as e_name, profiles.gender
           FROM users // main table
           LEFT JOIN profiles // sub table
           ON users.id = profiles.user_id
```

**->To show all rows from sub table or right table and if there matching relation from Main table (Right Join):**
```sql
           SELECT users.id, users.name, profiles.profile_name as e_name, profiles.gender
           FROM users // main table
           RIGHT JOIN profiles // sub table
```

**->To show some rows with condition from main table or left table and if there matching relation from sub table (left Join with Condition):**
```sql
           SELECT users.id, users.name, profiles.profile_name as e_name, profiles.gender
           FROM users // main table
           LEFT JOIN profiles // sub table
           ON users.id = profiles.user_id
           WHERE users.name = 'florencio'
```

**->To show some rows with condition from sub table or right table and if there matching relation from main table (right Join with Condition):**
```sql
            SELECT users.id, users.name, profiles.profile_name as e_name, profiles.gender
            FROM users // main table
            RIGHT JOIN profiles // sub table
            ON users.id = profiles.user_id
            WHERE profiles.gender = 'male'
```


**->To Show Specific row(s) in relationship:**
```sql
             SELECT users.name, profiles.name as profile_name, profiles.gender
             FROM users
             INNER JOIN profiles
             ON users.id = profiles.user_id
             WHERE profiles.name = 'elif'
                 // or
             WHERE profiles.user_id = '2'
```

----------------
### One to Many By SQL Query:

#### -=>The Example in Table Design:
```php
            =>table: users('id', 'name')
            =>table: posts('id', 'name', 'gender', 'user_id')
            =>table: profiles('id', 'name', 'gender', 'user_id') // optional
```

**->To show all rows in relation:**
```sql
            SELECT users.name, posts.title as post_name, posts.body
            FROM users
            INNER JOIN posts
            ON users.id = posts.user_id
```

**->To show all rows from main table or left table and if there matching relation from sub table (left Join):**
```sql
			SELECT users.id, users.name, posts.title as p_title, posts.body
            FROM users // main table
            LEFT JOIN posts // sub table
            ON users.id = posts.user_id
```

**->To show all rows from sub table or right table and if there matching relation from Main table (Right Join):**
```sql
           SELECT users.id, users.name, posts.title as p_title, posts.body
           FROM users // main table
           RIGHT JOIN posts // sub table
           ON users.id = posts.user_id
```

**->To show some rows with condition from main table or left table and if there matching relation from sub table (left Join with Condition):**
```sql
           SELECT users.id, users.name, posts.title as p_title, posts.body
           FROM users // main table
           LEFT JOIN posts // sub table
           ON users.id = posts.user_id
           WHERE users.name = 'florencio'
```
**->To show some rows with condition from sub table or right table and if there matching relation from main table (right Join with Condition):**
```sql
           SELECT users.id, users.name, posts.title as p_title, posts.body
           FROM users // main table
           RIGHT JOIN posts // sub table
           ON users.id = posts.user_id
           WHERE posts.body = 'bobo'
```

**->To Show Specific row(s) in relation with condition from two tables:**
```sql
SELECT users.name, users.id as userId, posts.title as post_name, posts.body, posts.id as postId
FROM users
INNER JOIN posts
ON users.id = posts.user_id
WHERE users.id = '2'

```

**->To Show Specific row(s) in relation with condition from three tables:**
```sql
SELECT users.name, users.id as userId, posts.title as post_name, posts.body, posts.id as postId, profiles.profile_name as pro_name
FROM users
INNER JOIN posts
ON users.id = posts.user_id
INNER JOIN profiles
ON users.id = profiles.user_id
WHERE users.id = '2'
```

-------------
### Many to Many By SQL Query:

#### -=>The Example in Table Design:
```php
        $table->unsignedBigInteger('product_id');
		$table->foreign('product_id')->references('id')->on('products')->onDelete('cascade');
		$table->unsignedBigInteger('order_id');
		$table->foreign('order_id')->references('id')->on('orders')->onDelete('cascade');
		$table->primary(['product_id', 'order_id']);
		        // we can add timestamps if need it.
		$table->timestamps();
```
**->To show all rows in relation:**
```sql
            SELECT  orders.*, products.*
            FROM orders
            INNER JOIN product_order
            ON orders.id = product_order.order_id
            INNER JOIN products
            ON products.id = product_order.product_id
```


**->To show all rows from table1 or left table and if there matching relation from table2 (left Join):**
```sql
             SELECT  orders.*, products.*
             FROM orders // table1
             LEFT JOIN product_order
             ON orders.id = product_order.order_id
             LEFT JOIN products // table2
             ON products.id = product_order.product_id
```


**->To show all rows from table2 or left table and if there matching relation from table1 (right Join):**
```sql
            SELECT  orders.*, products.*
            FROM orders // table1
            RIGHT JOIN product_order
            ON orders.id = product_order.order_id
            RIGHT JOIN products // table2
            ON products.id = product_order.product_id
```


**->To show some row(s) from table1 or left table and if there matching relation from table2 (left Join with condition):**
```sql
            SELECT  orders.*, products.*
            FROM orders // table1
            LEFT JOIN product_order
            ON orders.id = product_order.order_id
            LEFT JOIN products // table2
            ON products.id = product_order.product_id
            WHERE orders.id = '1'
                // or
            WHERE orders.name = 'nnn'
```


**->To show some row(s) from table2 or left table and if there matching relation from table1 (right Join with condition):**
```sql
           SELECT  orders.*, products.*
           FROM orders // table1
           RIGHT JOIN product_order
           ON orders.id = product_order.order_id
           RIGHT JOIN products // table2
           ON products.id = product_order.product_id
           WHERE products.id = '1'
               // or
           WHERE products.title = 'kkk'
```

**->To Show Specific row(s) in relation with condition:**
```sql
           SELECT  orders.*, products.*
           FROM orders
           INNER JOIN product_order
           ON orders.id = product_order.order_id
           INNER JOIN products
           ON products.id = product_order.product_id
           WHERE products.id = '2'
               // or
           WHERE products.title = 'nnn'
               // or
           WHERE orders.id = '5'
               // or
           WHERE orders.name = 'ppp'
```

========================================================
## By PHP Laravel Query Builder:

### -> One to One By Query Builder:

#### -=>The Example in Table Design:
```php
         =>table: users('id', 'name')
         =>table: profiles('id', 'name', 'gender')
  ```
**->To show all rows in relation:**
```php
         $user_profile = DB::table('users')
                 ->select('profiles.*', 'users.name as user_name')
                 ->join('profiles', 'users.id', '=', 'profiles.user_id')
                 ->get();
```


**->To show all rows from main table or left table and if there matching relation from sub table (left Join):**
```php
         $user_profile = DB::table('users')
                 ->select('profiles.*', 'users.name as user_name')
                 ->leftJoin('profiles', 'users.id', '=', 'profiles.user_id')
                 ->get();
```

**->To show all rows from sub table or right table and if there matching relation from Main table (Right Join):**
```php
         $user_profile = DB::table('users')
                 ->select('profiles.*', 'users.name as user_name')
                 ->rightJoin('profiles', 'users.id', '=', 'profiles.user_id')
                 ->get();
```

**->To show some rows with condition from main table or left table and if there matching relation from sub table (left Join with Condition):**
```php
         $user_profile = DB::table('users')
                 ->select('profiles.*', 'users.name as user_name')
                 ->leftJoin('profiles', 'users.id', '=', 'profiles.user_id')
                 ->where('users.id', '=', '5')
                 ->orWhere('users.name', '=', 'flirencio')
                 ->first(); // to one row
                     // or
                 ->get(); // to show all rows that match where condition
```

**->To show some rows with condition from sub table or right table and if there matching relation from main table (right Join with Condition):**
```php
          $user_profile = DB::table('users')
                  ->select('profiles.*', 'users.name as user_name')
                  ->rightJoin('profiles', 'users.id', '=', 'profiles.user_id')
                  ->where('profiles.user_id', '=', '5')
                  ->orWhere('profiles.gender', '=', 'male')
                  ->first(); // to one row
                      // or
                  ->get(); // to show all rows that match where condition
```



**->To Show Specific row(s) in relation with Condition:**
```php
         $user_profile = DB::table('users')
	              ->select('profiles.*', 'users.name as user_name')
	              ->join('profiles', 'users.id', '=', 'profiles.user_id')
	              ->where(users.id', '=', '5')
	              ->orWhere('profiles.name', '=', 'elif')
	              ->first(); // to one row
	                  // or
	              ->get(); // to show all rows that match where condition
```


**#to print in view:**

```php
                  - for check first() use:
                  @if(!empty($user_profile))
                      $user_profile->name
                      $user_profile->title
                  @endif
```
```php
                  - for check get() use:
                  we don't need check for get()
```
--------------------------------
### -> One to Many By Query Builder:

#### -=>The Example in Table Design:
```php
           =>table: users('id', 'name')
           =>table: posts('id', 'name', 'gender', 'user_id')
```


**->To show all rows in relation:**
```php
          $user_posts = DB::table('users')
                  ->select('posts.*', 'users.name as user_name')
                  ->join('posts', 'users.id', '=', 'posts.user_id')
                  ->get();
```

**->To show all rows from main table or left table and if there matching relation from sub table (left Join):**
```php
		  $user_posts = DB::table('users')
                  ->select('posts.*', 'users.name as user_name')
                  ->leftJoin('posts', 'users.id', '=', 'posts.user_id')
                  ->get();
```
**->To show all rows from sub table or right table and if there matching relation from Main table (Right Join):**
```php
	       $user_posts = DB::table('users')
                   ->select('posts.*', 'users.name as user_name')
                   ->rightJoin('posts', 'users.id', '=', 'posts.user_id')
                   ->get();
```

**->To show some rows with condition from main table or left table and if there matching relation from sub table (left Join with Condition):**
```php
            $user_posts = DB::table('users')
                    ->select('posts.*', 'users.name as user_name')
                    ->leftJoin('posts', 'users.id', '=', 'posts.user_id')
                    ->where('users.id', '=', '5')
                    ->orWhere('users.name', '=', 'florencio')
                    ->first(); // to one row
                        // or
                    ->get(); // to show all rows that match where condition
```

**->To show some rows with condition from sub table or right table and if there matching relation from main table (right Join with Condition):**
```php
             $user_posts = DB::table('users')
                     ->select('posts.*', 'users.name as user_name')
                     ->rightJoin('posts', 'users.id', '=', 'posts.user_id')
                     ->where('posts.title', '=', 'gogo')
                     ->orWhere('posts.user_id', '=', '2')
                     ->first(); // to one row
                         // or
                     ->get(); // to show all rows that match where condition
```


**->To Show Specific row(s) in relation with Condition:**
```php
              $user_posts = DB::table('users')
                      ->select('posts.*', 'users.name as user_name')
                      ->join('posts', 'users.id', '=', 'posts.user_id')
                      ->where('users.id', '=', '5')
                      ->orWhere('posts.name', '=', 'elif')
                      ->first(); // to one row
                          // or
                       ->get(); // to show all rows that match where condition
```

**#to print in view:**
```php
             - for check first() use:
             @if(!empty($user_posts))
                 $user_posts->name
                 $user_posts->title
             @endif
```
```php
	         - for check get() use:
	         we don't need check for get()
```

--------------------------------

### Many to Many By Query Builder:

#### -=>The Example in Tables Design:
```php
        $table->unsignedBigInteger('product_id');
		$table->foreign('product_id')->references('id')->on('products')->onDelete('cascade');
		$table->unsignedBigInteger('order_id');
		$table->foreign('order_id')->references('id')->on('orders')->onDelete('cascade');
		$table->primary(['product_id', 'order_id']);
		        // we can add timestamps if need it.
		$table->timestamps();
```

**->To show all rows in relation:**
```php
         $orders_products = DB::table('orders')
                 ->select('orders.*', 'products.*')
                 ->join('product_order', 'product_order.order_id', '=', 'orders.id')
                 ->join('products', 'products.id', '=', 'product_order.product_id')
                 ->get();
```


**->To show all rows from table1 or left table and if there matching relation from table2 (left Join):**
```php
         $orders_products = DB::table('orders')  // table1
                 ->select('orders.*', 'products.*')
                 ->leftJoin('product_order', 'product_order.order_id', '=', 'orders.id')
                 ->leftJoin('products', 'products.id', '=', 'product_order.product_id') // table2
                 ->get();
```


**->To show all rows from table2 or right table and if there matching relation from table1 (Right Join):**
```php
	       $orders_products = DB::table('orders')  // table1
	              ->select('orders.*', 'products.*')
	              ->rightJoin('product_order', 'product_order.order_id', '=', 'orders.id')
	              ->rightJoin('products', 'products.id', '=', 'product_order.product_id') // table2
	              ->get();
```


**->To show some rows with condition from main table or left table and if there matching relation from sub table (left Join with Condition):**
```php
 $orders_products = DB::table('orders')  // table1
         ->select('orders.*', 'products.*')
         ->leftJoin('product_order', 'product_order.order_id', '=', 'orders.id')
         ->leftJoin('products', 'products.id', '=', 'product_order.product_id') // table2
         ->where('orders.id', '=', '2')
         ->orWhere('orders.address', '=', '3 st florencio')
         ->first(); // to one row
             // or
         ->get(); // to show all rows that match where condition
```

**->To show some rows with condition from sub table or right table and if there matching relation from main table (right Join with Condition):**
```php
 $orders_products = DB::table('orders')  // table1
         ->select('orders.*', 'products.*')
         ->rightJoin('product_order', 'product_order.order_id', '=', 'orders.id')
         ->rightJoin('products', 'products.id', '=', 'product_order.product_id') // table2
         ->where('products.id', '=', '5')
         ->orWhere('products.title', '=', 'florencio')
         ->first(); // to one row
             // or
         ->get(); // to show all rows that match where condition
```

**->To Show Specific row(s) in relation with Condition:**
```php
              $orders_products = DB::table('orders')
                      ->select('orders.*', 'products.*')
                      ->join('product_order', 'product_order.order_id', '=', 'orders.id')
                      ->join('products', 'products.id', '=', 'product_order.product_id')
                      ->where('orders.id', '=', '5')
                      ->orWhere('products.title', '=', 'elif')
                      ->first(); // to one row
                          // or
                      ->get(); // to show all row that match where condition
```

**#to print in view:**
```php
               - for check first() use:
               @if(!empty($orders_products))
                   $orders_products->name
                   $orders_products->title
               @endif
```
```php
               - for check get() use:
               we don't need check for get()
```

========================================================
## By PHP Laravel Eloquent ORM:

**->general functions:**
The different between 'find()' and where('', '')  :-
- find() use in 'id' the column that has primaryKey attribute.
and when need show it use 'first()' not 'get()'

- where() use for any column and when need show it use
    'get()' for array, and 'first()' for show first
    column matching condition.
--------------------------
### One to One By Eloquent:
#### -=>The Example in Table Design:
```php
            =>table: users('id', 'name')
            =>table: profiles('id', 'name', 'gender', 'user_id')
```



**=> if we have the data of main table and need show data from main table and sub table:**
- create two model for the two tables.
- in the mode of the main table should create method with the name of sub table. like this:
```php
          public function profile()
          {
              return $this->hasOne(App\Profile, 'user_id');
          }

          - hasOne('model', foreign_key:optional, local_key:optional)
```
**Parameters:**
**1st parameter:(model)***: the root of sub table model, not optional.*
**2nd parameter:(foreign_key)***: the name of the column in sub table that should have a value matching the id (or the custom $primaryKey) column in main table, optional.
**Example:** 'user_id'*
**3rd parameter:(local_key)***: the name of column in main table that matching value in sub table, the default value is 'id',
should be $primaryKey.
**Example:** 'id'.*

**- in the Controller:**

**-> to show all rows from main table and if matching relation in sub table get row from sub table also (left Join):**
```php
         $user_profile = User::with('profile')->get();
```
***#to print in view:***
```php
                 @foreach($user_profile as $pp)
                     <h1>{{ $pp->email }}</h1>
                     @if(!empty($pp->profile->gender))
                         <h1> {{ $pp->profile->gender }}</h1>
                     @endif
                 @endforeach

```

**-> to show all rows in relation only:**
```php
                 $user_profile = User::has('profile')->with('profile')->get();
```

***#to print in view:***
```php
                   @foreach($user_profile as $pp)
                           <h1>{{ $pp->email }}</h1>
                           <h1>{{ $pp->profile->gender }}</h1>
                   @endforeach
```


**-> to show Specific rows from main table in relations only:**
```php
$user_profile = User::has('profile')->where('name', 'florencio')->with('profile')->get();
```
***#to print in view:***
```php
                      @foreach($user_profile as $pp)
                             <h1>{{ $pp->email }}</h1>
                             <h1>{{ $pp->profile->gender }}</h1>
                      @endforeach
```

**-> to show Specific rows from main table and if matching relation in sub table get row from sub table (left Join):**
```php
$user_profile = User::where('name', 'florencio')->with('profile')->get();
```

***#to print in view:***
```php
                      @foreach($user_profile as $pp)
                          <h1>{{ $pp->email }}</h1>
                          @if(!empty($pp->profile->gender))
                              <h1>{{ $pp->profile->gender }}</h1>
                          @endif
                      @endforeach
```

**-> to show Specific one row from main table and if matching relation in sub table get row from sub table (left Join):**
```php
                      $user_profile = User::findOrfail('2');
```
***#to print in view:***
```php
                      {{ $user_profile->email }}
                      @if(!empty($user_profile->profile->gender))
                          {{ $user_profile->profile->gender }}
                      @endif
```
--------------------------
### Inverse One To One By Eloquent:

**=> if we have the data of sub table and need show data from main table and sub table:**
- create two model for the two tables
- in the model of the sub table should create method with the name of main table. like this:
```php
          public function user()
          {
              return $this->belongsTo('App\User');
          }

         - belongsTo('model', foreign_key:optional, other_key:optional)
```
**Parameters:**
**1st parameter:(model)***: the root of main table model, not optional.*
**2nd parameter:(foreign_key)***: the name of the column in sub table that should have a value matching the id (or the custom $primaryKey) column in main table, optional.*
 **Example***: 'user_id'*

**3rd parameter:(other_key)***: the name of column in main table that matching value in sub table, the default value is 'id',   should be $primaryKey.*
**Example***: 'id'.*

  **- in the Controller:**

**-> to show all rows from sub table and if matching relation in main table get row from main table also (right Join):**
```php
             `$user_profile = Profile::with('user')->get();
```
***#to print in view:***
```php
                @foreach($user_profile as $pp)
                     <h1>{{ $pp->gender }}</h1>
                     @if(!empty($pp->user))
                             <h1>{{ $pp->user->email }}</h1>
                     @endif
                @endforeach
```

 **-> to show all rows in relation only:**
```php
            $user_profile = Profile::has('user')->with('user')->get();
```
  ***#to print in view:***
```php
             @foreach($user_profile as $pp)
                 <h1>{{ $pp->gender }}</h1>
                 <h1>{{ $pp->user->email }}</h1>
             @endforeach
```

  **-> to show Specific rows from sub table which in relations only:**
```php
$user_profile = Profile::has('user')->where('gender', 'male')->with('user')->get();
```
  ***#to print in view:***
```php
               @foreach($user_profile as $pp)
                       <h1>{{ $pp->gender }}</h1>
                       <h1>{{ $pp->user->email }}</h1>
               @endforeach
```



**-> to show Specific rows from sub table and if matching relation in main table get row from main table (right Join):**
```php
$user_profile = Profile::where('gender', 'male')->with('user')->get();
```
***#to print in view:***
```php
            @foreach($user_profile as $pp)
                {{ $pp->gender }}
                @if(!empty($pp->user))
                    {{ $pp->user->email }}
                @endif
            @endforeach
```

**-> to show Specific one row from sub table and if matching relation in main table get row from main table also (right Join):**
```php
            $user_profile = Profile::findOrfail('2');
```
***#to print in view:***
```php
            {{ $user_profile->gender }}
            @if(!empty($user_profile->user))
                {{ $user_profile->user->email }}
            @endif
```


--------------------------------

### One to One Create Relation and Update Relation and Remove Relation:


>  **- from Main table:**


**- Insert new row with relation with check:**
```php
            $user = App\User::find(1);
            $user->posts()->create([
                'title' => 'A new title.',
                'body' => 'A new content'
            ]);
```

> **- from sub table:**


**- Insert new row with relation:**
```php
              App\Profile::create([
                   'title' => 'A new title.',
                   'body' => 'A new content',
                   user_id => '1'
               ]);
```
**- Update relation:**
```php
			  App\Profile::find(1)->update(['user_id' => 5]);
```

**- Update relation with check:**
```php
             $profile = Profile::find(13);
             $user = User::find(1);
             $profile->user()->associate($user);
             $profile->save();
```
**- Remove Relation:**
```php
            Profile::find(11)->update(['user_id' => null]);
                    // or
            $profile = Profile::find(11);
            $profile->user()->dissociate();
            $profile->save();
```
 --------------------------------
### One to Many By Eloquent:
#### -=>The Example in Table Design:
```php
            =>table: users('id', 'name')
            =>table: posts('id', 'name', 'title', 'body', 'user_id')
```
**=> if we have the data of main table and need show data from main table and sub table:**
- create two model for the two tables
- in the mode of the main table should create method with the name of sub table. like this:
```php
          public function posts()
          {
              return $this->hasMany('App\Post', 'user_id');
          }

          - hasOne('model', foreign_key:optional, local_key:optional)
```
**Parameters:**
**1st parameter:(model)***: the root of sub table model, not optional.*
**2nd parameter:(foreign_key)***: the name of the column in sub table that should have a value matching the id (or the custom $primaryKey) column in main table, optional.*
**Example:** 'user_id'
**3rd parameter:(local_key)***: the name of column in main table that matching value in sub table, the default value is 'id',  should be $primaryKey.*
**Example:** 'id'.

**- in the Controller:**

**-> to show all rows from main table and if matching relation in sub table get row from sub table also (left Join):**
```php
           $user_posts = User::with('posts')->get();
```
***#to print in view:***
```php
            @foreach($user_posts as $item)
                {{ $item->email }}
                {{ $item->name }}
                    @foreach($item->posts as $post)
                        {{ $post->title }}
                        {{ $post->body }}
                    @endforeach
            @endforeach
```

**-> to show all rows in relation only:**
```php
          $user_posts = User::has('posts')->with('posts')->get();
```
***#to print in view:***
```php
           @foreach($user_posts as $item)
               {{ $item->email }}
               {{ $item->name }}
                   @foreach($item->posts as $post)
                       {{ $post->title }}
                       {{ $post->body }}
                   @endforeach
           @endforeach
```

**-> to show Specific rows from main table in relations only:**
```php
      $user_posts = User::has('posts')->where('name', 'florencio')->with('posts')->get();
```
***#to print in view:***
```php
       @foreach($user_posts as $item)
           {{ $item->email }}
           {{ $item->name }}
               @foreach($item->posts as $post)
                   {{ $post->title }}
                   {{ $post->body }}
               @endforeach
       @endforeach
```


**-> to show Specific rows from main table and if matching relation in sub table get row from sub table (left Join):**
```php
$user_posts = User::where('name', 'florencio')->with('posts')->get();
```
***#to print in view:***
```php
         @foreach($user_posts as $item)
             {{ $item->email }}
             {{ $item->name }}
                 @foreach($item->posts as $post)
                     {{ $post->title }}
                     {{ $post->body }}
                 @endforeach
         @endforeach
```

**-> to show Specific one row from main table and if matching relation in sub table get row from sub table (left Join):**
```php
        $user_posts = User::findOrfail('2');
```
***#to print in view:***
```php
         {{ $user_posts->email }}
         @foreach($user_posts->posts as $post)
             {{ $post->title }}
             {{ $post->body }}
         @endforeach
```

  ----------------------------
### Inverse One To Many By Eloquent:

**=> if we have the data of sub table and need show data from main table and sub table:**
- create two model for the two tables
- in the model of the sub table should create method with the name of main table. like this:
```php

               public function user()
               {
                   return $this->belongsTo('App\Post', 'user_id');
               }
             - belongsTo('model', foreign_key:optional, other_key:optional)
```
**Parameters:**
**1st parameter:(model)***: the root of main table model, not optional.*
**2nd parameter:(foreign_key)***: the name of the column in sub table that should have a value matching  the id (or the custom $primaryKey) column in main table, optional.*
**Example:** *'user_id'*
**3rd parameter:(other_key)***: the name of column in main table that matching value in sub table, the default value is 'id',  should be $primaryKey.*
**Example:** *'id'.*

**- in the Controller:**

**-> to show all rows from sub table and if matching relation in main table get row from main table also (right Join):**
```php
           $user_posts = Post::with('user')->get();
```
***#to print in view:***
```php
           @foreach($user_posts as $post)
                <h1>{{ $post->title }}</h1>
                <h1>{{ $post->body }}</h1>
                @if(!empty($post->user))
                        <h1>{{ $post->user->email }}</h1>
                @endif
           @endforeach
```

**-> to show all rows in relation only:**
```php
         $user_posts = Post::has('user')->with('user')->get();
```
***#to print in view:***
```php
           @foreach($user_posts as $post)
               <h1>{{ $post->title }}</h1>
               <h1>{{ $post->body }}</h1>
               <h1>{{ $post->user->email }}</h1>
           @endforeach
```
**-> to show Specific rows from sub table which in relations only:**
```php
          $user_posts = Post::has('user')->where('title', 'zzz')->with('user')->get();
```
***#to print in view:***
```php
           @foreach($user_posts as $post)
                   <h1>{{ $post->title }}</h1>
                   <h1>{{ $post->body }}</h1>
                   <h1>{{ $post->user->email }}</h1>
           @endforeach
```



**-> to show Specific rows from sub table and if matching relation in main table get row from main table (right Join):**
```php
          $user_posts = Post::where('title', 'zzz')->with('user')->get();
```
***#to print in view:***
```php
           @foreach($user_posts as $post)
               {{ $post->title }}
               {{ $post->body }}
               @if(!empty($post->user))
                   {{ $post->user->email }}
                   {{ $post->user->name }}
               @endif
           @endforeach
```


**-> to show Specific one row from sub table and if matching relation in main table get row from main table also (right Join):**
```php
           $user_posts = Post::findOrfail('2');
```
***#to print in view:***
```php
           {{ $user_posts->title }}
           {{ $user_posts->body }}
           @if(!empty($user_posts->user))
               {{ $user_posts->user->email }}
               {{ $user_posts->user->name }}
           @endif
```
--------------------------------
### One to Many Create Relation and Update Relation and Remove Relation:

>  - from Main table:
>
**- Insert new row with relation with check:**
```php
       $user = App\User::find(1);
       $user->posts()->create([
           'title' => 'A new title.',
           'body' => 'A new content'
       ]);
```

**- Insert new rows with relation with check:**
```php
        $user = App\User::find(1);
        $user->posts()->createMany([
              [
                  'title' => 'A new title.',
                  'body' => 'A new content'
              ],
              [
                  'title' => 'A new title.',
                  'body' => 'A new content'
              ],
        ]);
```
> - from sub table:

**- Insert new row with relation:**
```php
          App\Post::create([
               'title' => 'A new title.',
               'body' => 'A new content',
               user_id => '1'
           ]);
```
**- Update relation:**
```php
          App\Post::find(1)->update(['user_id' => 5]);

```
**- Update relation with check:**
```php
         $post = post::find(13);
         $user = User::find(1);
         $post->user()->associate($user);
         $post->save();
```



 **- Remove Relation:**
```php
         Post::find(11)->update(['user_id' => null]);
                 // or
         $post = Post::find(11);
         $post->user()->dissociate();
         $post->save();
```

--------------------------------

### Many To Many By Eloquent:

#### -=>The Example in Table Design:
```php
		$table->unsignedBigInteger('product_id');
		$table->foreign('product_id')->references('id')->on('products')->onDelete('cascade');
		$table->unsignedBigInteger('order_id');
		$table->foreign('order_id')->references('id')->on('orders')->onDelete('cascade');
		$table->primary(['product_id', 'order_id']);
		        // we can add timestamps if need it.
		$table->timestamps();
```

 **=> if we have the data of main table and need show data from main table and sub table:**
 - create Intermediate table that has two columns references from two main tables.
 - create two model for the two main tables.
 - in model of the each main table should create method with the name of the other table. like this:
```php
class Order extends Model
{
      public function products()
      {
          return $this->belongsToMany('App\Product', 'order_product', 'order_id', 'product_id');
              // if need timestamps() should be like this:
          return $this->belongsToMany('App\Product', 'order_product', 'order_id', 'product_id')->withTimestamps();

      }
}

- belongsToMany('model', Intermediate_Table:optional, foreign_key_for_the_local_model:optional, foreign_key_for_the_other_model:optional)
```
 **Parameters**:
 **1st parameter:(model)**: *the root of other model, not optional.*
 **2nd parameter:(Intermediate_Table)**: *the name of the table that should have the two columns are $primaryKey and references from two tables, optional.*
**Example:** *'order_product'*
**3rd parameter:(foreign_key_for_the_local_model)***: the name of column in Intermediate Table that references the table of local Model, optional.*
**Example:** *'order_id'.*
**4th parameter:(foreign_key_for_the_other_model)***: the name of column in Intermediate Table that references the other table, optional.*
**Example:** *'product_id'.*

**- in the Controller:**

**-> to show all rows from table and if matching relation in other table get row from other table also (left Join):**
```php
         $order_product = Order::with('products')->get();
```
***#to print in view:***
```php
         @foreach($order_product as $item)
             {{ $item->name }}
             {{ $item->number }}
                 @foreach($item->products as $itemProduct)
                     {{ $itemProduct->title }}
                     {{ $itemProduct->description }}
                             // if need show Timestamps
                     {{ $itemProduct->pivot->created_at }}
                 @endforeach
         @endforeach
```

**-> to show all rows in relation only:**
```php
         $order_product = Order::has('products')->with('products')->get();
```
***#to print in view:***
```php
           @foreach($order_product as $item)
               {{ $item->name }}
               {{ $item->number }}
                   @foreach($item->products as $itemProduct)
                       {{ $itemProduct->title }}
                       {{ $itemProduct->description }}
                               // if need show Timestamps
                       {{ $itemProduct->pivot->created_at }}
                   @endforeach
           @endforeach
```
**-> to show Specific rows from table in relations only:**
```php
$order_product = Order::has('products')->where('name', 'nnn')->with('products')->get();
```
***#to print in view:***
```php
          @foreach($order_product as $item)
              {{ $item->name }}
              {{ $item->number }}
                  @foreach($item->products as $itemProduct)
                      {{ $itemProduct->title }}
                      {{ $itemProduct->description }}
                              // if need show Timestamps
                      {{ $itemProduct->pivot->created_at }}
                  @endforeach
          @endforeach
```

**-> to show Specific rows from table and if matching relation in other table get row from other table (left Join):**
```php
$order_product = Order::where('name', 'nnn')->with('products')->get();
```
***#to print in view:***
```php
		 @foreach($order_product as $item)
		     {{ $item->name }}
		     {{ $item->number }}
		         @foreach($item->products as $itemProduct)
		             {{ $itemProduct->title }}
		             {{ $itemProduct->description }}
		                     // if need show Timestamps
		             {{ $itemProduct->pivot->created_at }}
		         @endforeach
		 @endforeach
```
**-> to show Specific one row from table and if matching relation in other table get row from other table (left Join):**
```php
            $order_product = Order::findOrfail('2');
```
***#to print in view:***
```php
             {{ $order_product->number }}
             @foreach($order_product->products as $itemProduct)
                 {{ $itemProduct->title }}
                 {{ $itemProduct->description }}
                         // if need show Timestamps
                 {{ $itemProduct->pivot->created_at }}
             @endforeach
```
--------------------------------

**- There is no Inverse in Many to Many Relationship, but we can make any of the two tables as the main table in relation.**
 --------------------------------
### Many to Many Create and Update and Remove Relation:

**-> Create/Update Relation:**

**---> syncWithoutDetaching () // add relation Without make Detach for other and if exactly relation exist before don't duplicate it.**

***- for one record:***
```php
        $product_id = request()->product_id;
        $order = App\Order::find(1);
        $order->products()->syncWithoutDetaching ($product_id);
```
***- for one record with insert data to other columns in intermediate table:***
```php
       $title = 'florencio';
       $product_id = request()->product_id;
       $order = App\Order::find(1);
       $order->products()->syncWithoutDetaching ($product_id, ['title' => $title]);
```

***- for multiple records:***
```php
        $order = App\Order::find(1);
        $order->products()->syncWithoutDetaching ([1, 2, 3, 4]);
```

***- for multiple records with insert data to other columns in intermediate table:***
```php
          $order = App\Order::find(1);
          $order->products()->syncWithoutDetaching ([
                     1 => ['title' => $title1],
                     2 => ['title' => $title2],
                     3 => ['title' => $title3]
                 ]);
```

**---> sync() // remove all exist relation and add the new relation.**
***- for one record:***
```php
          $product_id = request()->product_id;
          $order = App\Order::find(1);
          $order->products()->sync($product_id);
```
***- for one record with insert data to other columns in intermediate table:***
```php
           $title = 'florencio';
           $product_id = request()->product_id;
           $order = App\Order::find(1);
           $order->products()->sync($product_id, ['title' => $title]);
```

***- for multiple records:***
```php
           $order = App\Order::find(1);
           $order->products()->sync([1, 2, 3, 4]);
```

***- for multiple records with insert data to other columns in intermediate table:***
```php
            $order = App\Order::find(1);
            $order->products()->sync([
                       1 => ['title' => $title1],
                       2 => ['title' => $title2],
                       3 => ['title' => $title3]
                   ]);
```

**---> attach() // if the new relation exist before return error.**

***- for one record:***
```php
             $product_id = request()->product_id;
             $order = App\Order::find(1);
             $order->products()->attach($product_id);
```
***- for one record with insert data to other columns in intermediate table:***
```php
             $title = 'florencio';
             $product_id = request()->product_id;
             $order = App\Order::find(1);
             $order->products()->attach($product_id, ['title' => $title]);
```
***- for multiple records:***
```php
             $order = App\Order::find(1);
             $order->products()->attach([1, 2, 3, 4]);
```
***- for multiple records with insert data to other columns in intermediate table:***
```php
             $order = App\Order::find(1);
             $order->products()->attach([
                        1 => ['title' => $title1],
                        2 => ['title' => $title2],
                        3 => ['title' => $title3]
                    ]);
```
**-> Remove Relation:**

***- remove all relation of this record:***
```php
             $order = App\Order::find(1);
             $order->products()->detach();
```
***- remove specific one relation of this record:***
```php
             $product_id = request()->product_id;
             $order = App\Order::find(1);
             $order->products()->detach($product_id);
```
***- remove specific some relation of this record:***
```php
             $order = App\Order::find(1);
             $order->products()->detach([ 1, 2, 3 ]);
```
