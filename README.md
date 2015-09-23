## Databasics

Fork this repository into your own github profile.

Before closing the homework assignment issue:

- Update this README as follows:
  - [ ] For each question (just after or below the question itself) include the SQL you wrote to generate the answer
  - [ ] For each question (just after or below the question itself) include the *answer* to the question asked.

- Also:
  - [ ] Commit the updated `store.sqlite3` database file
  - [ ] Include a link to your forked repo in the comments when closing the issue.


NOTE: To run sqlite with this database: `sqlite3 store.sqlite3`

NOTE: You may want to keep a backup of the `store.sqlite3` file in case you damage the file. *OR* you can use the command `git checkout store.sqlite3` to undo any changes to the database file since the fork or your last commit.

## Explorer Mode

- [ ] How many users are there?
-     select count(first_name) from  users;
-     50
- [ ] What are the 5 most expensive items?
-     select * from items order by price desc limit 5;
-     25|Small Cotton Gloves|Automotive, Shoes & Beauty|Multi-layered modular service-desk|9984
-      83|Small Wooden Computer|Health|Re-engineered fault-tolerant adapter|9859
-     100|Awesome Granite Pants|Toys & Books|Upgradable 24/7 access|9790
-      40|Sleek Wooden Hat|Music & Baby|Quality-focused heuristic info-mediaries|9390
-      60|Ergonomic Steel Car|Books & Outdoors|Enterprise-wide secondary firmware|9341
- [ ] What's the cheapest book? (Does that change for "category is exactly 'book'" versus "category contains 'book'"?)
-     for the Books category select * from items where category = 'Books' order by price limit 1;
-     76|Ergonomic Granite Chair|Books|De-engineered bi-directional portal|1496
-     for categories containing 'books' select * from items where category like '%Books%' order by price limit 1;
-     76|Ergonomic Granite Chair|Books|De-engineered bi-directional portal|1496
- [ ] Who lives at "6439 Zetta Hills, Willmouth, WY"? Do they have another address?
-     select first_name, last_name, street, city, state from [users] join addresses on [users].id = user_id where street = "6439       Zetta Hills"
-     Corrine|Little|6439 Zetta Hills|Willmouth|WY
-     select first_name, last_name, street, city, state from [users] join addresses on [users].id = addresses.user_id where           first_name = "Corrine";
-     Corrine|Little|6439 Zetta Hills|Willmouth|WY
-     Corrine|Little|54369 Wolff Forges|Lake Bryon|CA
- [ ] Correct Virginie Mitchell's address to "New York, NY, 10108".
-     update addresses set city = "New York",  zip = "10108"  where street = "12263 Jake Crossing";
-     update addresses set city = "New York", state = "NY",  zip = "10108"  where street = "83221 Mafalda Canyon";
- [ ] How much would it cost to buy one of each tool?
-     select sum(price) from items where category =  "Tools";
-     7383
-     select sum(price) from items where category like  "%Tool%";
-     46477
- [ ] How many total items did we sell
-     select sum(quantity) from orders;
-     2125
- [ ] How much was spent on books?
-     select sum(price * quantity) from [items] join orders on [items].id = item_id where category like "%books%";
-     1081352
- [ ] Simulate buying an item by inserting a User for yourself and an Order for that User.
## Adventure Mode

- [ ] What item was ordered most often? Grossed the most money?
-     select items.id, title, category, price, sum(quantity) from [items] join orders on [items].id = item_id group by item_id        order by sum(quantity) desc limit 1;
-     65|Incredible Granite Car|Music, Sports & Clothing|7295|72
-     select items.id, title, category, price, sum(quantity * price) from [items] join orders on [items].id = item_id group by        item_id order by sum(quantity * price) desc limit 1;
-     65|Incredible Granite Car|Music, Sports & Clothing|7295|525240
- [ ] What user spent the most?
-     select user_id, sum(quantity * price) from [items] join orders on [items].id = item_id group by user_id order by                sum(quantity * price) desc limit 1;
-     19|639386
-     select * from users where id = 19;
-     19|Hassan|Runte|weston.kautzer@hoppe.biz
- [ ] What were the top 3 highest grossing categories?
-     select category, sum(quantity * price) from [items] join orders on [items].id = item_id group by category  order by      -       sum(quantity * price) desc limit 3;
-     Music, Sports & Clothing|525240
-     Beauty, Toys & Sports|449496
-     Sports|448410
