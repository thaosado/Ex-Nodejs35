CREATE DATABASE ex_b3;
USE ex_b3;

CREATE TABLE food_type(
	type_id INT PRIMARY KEY AUTO_INCREMENT,
	type_name VARCHAR(50)
);

CREATE TABLE food(
	food_id INT PRIMARY KEY AUTO_INCREMENT,
	food_name VARCHAR(100),
	image VARCHAR(250),
	price FLOAT,
	description VARCHAR(350),
	type_id INT,
	FOREIGN KEY (type_id) REFERENCES food_type(type_id)
);

CREATE TABLE order_order(
	user_id INT PRIMARY KEY AUTO_INCREMENT,
	food_id INT,
	amount INT,
	code VARCHAR(50),
	arr_sub_id VARCHAR(50),
	FOREIGN KEY (food_id) REFERENCES food(food_id)
);

CREATE TABLE sub_food(
	sub_id INT PRIMARY KEY AUTO_INCREMENT,
	sub_name VARCHAR(100),
	sub_price FLOAT,
	food_id INT,
	FOREIGN KEY (food_id) REFERENCES food(food_id)
);

CREATE TABLE user_user(
	user_id INT PRIMARY KEY AUTO_INCREMENT,
	full_name VARCHAR (100),
	email VARCHAR(50),
	password VARCHAR(30)
);

CREATE TABLE restaurant(
	res_id INT PRIMARY KEY AUTO_INCREMENT,
	res_name VARCHAR(50),
	image VARCHAR(250),
	description VARCHAR(350)
);

CREATE TABLE rate_res(
	user_id INT,
	res_id INT,
	amount INT,
	date_rate DATETIME,
	FOREIGN KEY (user_id) REFERENCES user_user(user_id),
	FOREIGN KEY (res_id) REFERENCES restaurant(res_id)
);

CREATE TABLE like_res(
	user_id INT,
	res_id INT,
	date_like DATETIME,
	FOREIGN KEY (user_id) REFERENCES user_user(user_id),
	FOREIGN KEY (res_id) REFERENCES restaurant(res_id)
);

-- Inserting data into the food_type table
INSERT INTO food_type (type_name)
VALUES ('Italian'), ('Mexican'), ('Chinese'),('VietNam'),('ThaiLand'), ('Campuchia');


-- Inserting data into the food table
INSERT INTO food (food_name, image, price, description, type_id)
VALUES ('Pizza', 'pizza.jpg', 9.99, 'Delicious cheese and toppings on a thin crust', 1),
       ('Taco', 'taco.jpg', 4.99, 'Crispy tortilla filled with seasoned meat and vegetables', 2),
       ('Kung Pao Chicken', 'kungpao.jpg', 12.99, 'Spicy stir-fried chicken with peanuts and vegetables', 3);

-- Inserting data into the order_order table
INSERT INTO order_order (food_id, amount, code, arr_sub_id)
VALUES (1, 2, 'ABC123', 'XYZ789'),
       (2, 3, 'DEF456', 'PQR321');

-- Inserting data into the sub_food table
INSERT INTO sub_food (sub_name, sub_price, food_id)
VALUES ('Extra Cheese', 1.99, 1),
       ('Guacamole', 0.99, 2);

-- Inserting data into the user_user table
INSERT INTO user_user (full_name, email, password)
VALUES ('John Doe', 'john@example.com', 'password1'),
       ('Jane Smith', 'jane@example.com', 'password2'),('Thanh Anh', 'thanhanh@example.com', 'password1'),
       ('Chan Hoang', 'chanhoang@example.com', 'password2'),
       ('Chau Tan', 'chautan@example.com', 'password1'),
       ('Nhu Y', 'nhuy@example.com', 'password2'),
       ('Sa', 'sa@example.com', 'password2');
-- Inserting data into the restaurant table
INSERT INTO restaurant (res_name, image, description)
VALUES ('Pizzeria Italia', 'italia.jpg', 'Authentic Italian pizza and pasta'),
       ('Taco Fiesta', 'fiesta.jpg', 'Vibrant Mexican flavors in every bite');

-- Inserting data into the rate_res table
INSERT INTO rate_res (user_id, res_id, amount, date_rate)
VALUES (1, 1, 5, '2023-09-01 10:15:00'),
       (2, 2, 4, '2023-09-02 14:30:00'),
       (3, 1, 5, '2023-09-01 10:05:00'),
       (2, 2, 4, '2023-09-02 14:20:00'),
       (4, 1, 5, '2023-09-01 11:15:00'),
       (6, 2, 4, '2023-09-02 04:30:00'),
       (3, 1, 5, '2023-09-01 12:15:00'),
       (5, 2, 4, '2023-09-02 15:10:00');
       

-- Inserting data into the like_res table
INSERT INTO like_res (user_id, res_id, date_like)
VALUES (1, 2, '2023-09-03 18:45:00'),
       (2, 1, '2023-09-04 09:00:00'),
       (4, 2, '2023-09-03 18:45:00'),
       (5, 1, '2023-09-04 09:00:00'),
       (4, 2, '2023-09-03 18:45:00'),
       (3, 2, '2023-09-04 09:00:00');
       
---Tìm 5 người đã like nhà hàng nhiều nhất

SELECT COUNT(lr.user_id) as count_like_user, u.full_name, u.email, lr.user_id
FROM like_res as lr
INNER JOIN user_user as u on lr.user_id = u.user_id
GROUP BY u.full_name, u.full_name, lr.user_id
ORDER BY COUNT(lr.user_id) DESC
limit 5

--- Tìm 2 nhà hàng có lượt like nhiều nhất
SELECT COUNT(lr.res_id) as count_like_res, r.res_name, r.image, lr.res_id
FROM like_res as lr
INNER JOIN restaurant as r on lr.res_id = r.res_id
GROUP BY r.res_name, r.image, lr.res_id
ORDER BY COUNT(lr.res_id) DESC
limit 2;

---Tìm người đã đặt hàng nhiều nhất
SELECT COUNT(od.user_id) as count_user_id, u.full_name, u.email, od.user_id
FROM order_order as od
INNER JOIN user_user as u on od.user_id = u.user_id
GROUP BY u.full_name, u.email, od.user_id
ORDER BY COUNT(od.user_id) DESC
limit 1;

---Tìm người dùng không hoạt động trong hệ thống
SELECT * FROM user_user as u
LEFT JOIN order_order as o ON o.user_id = u.user_id
LEFT JOIN like_res as lr ON lr.user_id = u.user_id
LEFT JOIN rate_res as rr ON rr.user_id = u.user_id
WHERE o.user_id IS NULL && lr.user_id IS NULL && rr.user_id IS NULL

