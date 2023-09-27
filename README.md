CREATE DATABASE bc35_sql;

USE bc35_sql;

CREATE TABLE users(
	user_id INT PRIMARY KEY AUTO_INCREMENT,
	full_name VARCHAR(100),
	email VARCHAR(100),
	password VARCHAR(100)
)

CREATE TABLE restaurant(
	res_id INT PRIMARY KEY AUTO_INCREMENT,
	res_name VARCHAR(100),
	image VARCHAR(300),
	descs VARCHAR(100)
)

CREATE TABLE rate_res(
	amount INT,
	date_rate VARCHAR(100),
	user_id INT,
	FOREIGN KEY (user_id) REFERENCES users(user_id),
	
	res_id INT,
	FOREIGN KEY (res_id) REFERENCES restaurant(res_id)
	
)

CREATE TABLE like_res(
	
	date_like VARCHAR(100),
	
	res_id INT,
	FOREIGN KEY (res_id) REFERENCES restaurant(res_id),
	
	user_id INT,
	FOREIGN KEY (user_id) REFERENCES users(user_id)
)

DROP TABLE like_res
CREATE TABLE food_type(
	type_id INT PRIMARY KEY AUTO_INCREMENT,
	type_name VARCHAR(100)
)

CREATE TABLE oder(
	amount INT,
	code VARCHAR(100),
	arr_sub_id VARCHAR(100),
	user_id INT,
	FOREIGN KEY (user_id) REFERENCES users(user_id),
	
	food_id INT,
	FOREIGN KEY (food_id) REFERENCES food(food_id)
)

CREATE TABLE food(
	food_id INT PRIMARY KEY AUTO_INCREMENT,
	food_name VARCHAR(100),
	image VARCHAR(300),
	price FLOAT,
	description VARCHAR(100),
	type_id INT,
	FOREIGN KEY(type_id) REFERENCES food_type(type_id)
)

CREATE TABLE sub_food(
	sub_id INT PRIMARY KEY AUTO_INCREMENT,
	sub_name VARCHAR(100),
	sub_price FLOAT,
	food_id INT,
	FOREIGN KEY (food_id) REFERENCES food(food_id)
)

INSERT INTO users (full_name, email, password) 
VALUES ('John Doe', 'johndoe@example.com', 'password123'),
       ('Jane Smith', 'janesmith@example.com', 'pass456'),
       ('Tom Johnson', 'tomjohnson@example.com', 'qwerty');
       
INSERT INTO restaurant (res_name, image, descs) 
VALUES ('Nhà hàng ABC', 'abc.jpg', 'Nhà hàng sang trọng phục vụ các món ăn ngon'),
       ('Quán XYZ', 'xyz.jpg', 'Quán ăn nhanh với thực đơn đa dạng'),
       ('Quán PQR', 'pqr.jpg', 'Quán nhỏ xinh phục vụ ẩm thực địa phương');
       
INSERT INTO food_type (type_id, type_name) 
VALUES (1,'Món Tráng Miệng'),
       (2,'Món Chính'),
       (3,'Món Hấp');
       
INSERT INTO rate_res(user_id, res_id, amount, date_rate)
VALUES (2,1,9,"20/09/2023"),
	   (2,2,9,"23/09/2023"),
	   (3,3,9,"27/09/2023");
	   
INSERT INTO food(food_id, food_name, image, price, description, type_id)
VALUES (1,"Cơm gà","ABC",90000,"Ngon",2),
	   (2,"Cơm bò","ABC",91000,"Ngon",2),
	   (3,"Cá hấp","ABCD",80000,"Ngon",3),
	   (4,"Kem","CAB",10000,"Ngon",1),
	   (5,"Trái cây","BCA",11000,"Ngon",1);

INSERT INTO sub_food(sub_id, sub_name, sub_price, food_id)
VALUES (1, "Tuyệt", 10, 1),
	   (2, "Tuyệt", 9, 3),
	   (3, "Tuyệt", 9, 4);
	   
INSERT INTO oder (user_id, food_id, amount, code, arr_sub_id) 
VALUES (2,2, 2, "HD01",01),
	   (3,4, 1, "HD02",02),
	   (3,5, 2, "HD03",03);

INSERT INTO like_res(user_id, res_id, date_like)
VALUES (2,1,"21/9/2023"),
	   (2,2,"22/9/2023"),
	   (3,3,"23/9/2023"),
	   (3,1,"24/9/2023"),
	   
---------------------------------------------------------------------------------------------
    --Tìm 5 người đã like nhà hàng nhiều nhất
SELECT COUNT(like_res.user_id) as count_like_user , users.full_name, users.email, like_res.user_id
FROM like_res  
INNER JOIN users on like_res.user_id = users.user_id
GROUP BY users.full_name,  users.email, like_res.user_id
ORDER BY COUNT(like_res.user_id) DESC
limit 5
	
	--Tìm 2 nhà hàng có lượt like nhiều nhất 
SELECT COUNT(like_res.res_id) as count_like_res, restaurant.res_name
FROM like_res
INNER JOIN restaurant on like_res.res_id = restaurant.res_id 
GROUP BY restaurant.res_name, like_res.res_id
ORDER BY COUNT(like_res.res_id) DESC
LIMIT 2

	 --Tìm người đặt hàng nhiều nhất
SELECT COUNT(users.user_id) as users_order, users.full_name
FROM oder
INNER JOIN users on oder.user_id = users.user_id 
GROUP BY users.full_name, oder.user_id
ORDER BY COUNT(oder.user_id) DESC
LIMIT 1

	 --Tìm người không hoạt động trong hệ thống(không đặt hàng, không like, không đánh giá nhà)

	   
	   
	   
