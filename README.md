# Triggers---question-1
mysql> CREATE TABLE teachers (    id INT PRIMARY KEY AUTO_INCREMENT,    name VARCHAR(50),    subject VARCHAR(50),    experience INT,    salary DECIMAL(10, 2));


mysql> INSERT INTO teachers (id, name, subject, experience, salary) VALUES
    -> (1, 'Unnikrishnan', 'Mathematics', 8, 50000.00),
    -> (2, 'Abijith', 'English', 12, 60000.00),
    -> (3, 'Harikrishnan', 'Science', 6, 55000.00),
    -> (4, 'Kavya', 'History', 10, 52000.00),
    -> (5, 'Walter White', 'Geography', 5, 48000.00),
    -> (6, 'Jesse', 'Physics', 15, 70000.00),
    -> (7, 'Hank', 'Chemistry', 9, 53000.00),
    -> (8, 'Anna Red', 'Biology', 7, 51000.00);

mysql> DELIMITER $$
mysql>
mysql> CREATE TRIGGER before_insert_teacher
    -> BEFORE INSERT ON teachers
    -> FOR EACH ROW
    -> BEGIN
    ->     IF NEW.salary < 0 THEN
    ->         SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Salary cannot be negative';
    ->     END IF;
    -> END $$
mysql>
mysql> DELIMITER ;
mysql>
mysql>
mysql> DELIMITER $$
mysql>
mysql> CREATE TRIGGER after_insert_teacher
    -> AFTER INSERT ON teachers
    -> FOR EACH ROW
    -> BEGIN
    ->     INSERT INTO teacher_log (teacher_id, action, log_timestamp)
    ->     VALUES (NEW.id, 'INSERT', NOW());
    -> END $$
mysql>
mysql> DELIMITER ;
mysql>
mysql>
mysql> DELIMITER $$
mysql>
mysql> CREATE TRIGGER before_delete_teacher
    -> BEFORE DELETE ON teachers
    -> FOR EACH ROW
    -> BEGIN
    ->     IF OLD.experience > 10 THEN
    ->         SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Cannot delete a teacher with more than 10 years of experience';
    ->     END IF;
    -> END $$

mysql>
mysql> DELIMITER ;
mysql>
mysql>
mysql> DELIMITER $$
mysql>
mysql> CREATE TRIGGER after_delete_teacher
    -> AFTER DELETE ON teachers
    -> FOR EACH ROW
    -> BEGIN
    ->     INSERT INTO teacher_log (teacher_id, action, log_timestamp)
    ->     VALUES (OLD.id, 'DELETE', NOW());
    -> END $$
mysql>
mysql> DELIMITER ;
mysql> ss



