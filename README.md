# Triggers---question-1
1. Create a table named teachers with fields id, name, subject, experience and salary and insert 8 rows. 

DROP TABLE IF EXISTS teachers;

CREATE TABLE teachers (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    subject VARCHAR(50),
    experience INT,
    salary DECIMAL(10, 2) CHECK (salary >= 0)
);

INSERT INTO teachers (id, name, subject, experience, salary) VALUES
(1, 'Unnikrishnan', 'Mathematics', 8, 50000.00),
(2, 'Abijith', 'English', 12, 60000.00),
(3, 'Harikrishnan', 'Science', 6, 55000.00),
(4, 'Kavya', 'History', 10, 52000.00),
(5, 'Walter White', 'Geography', 5, 48000.00),
(6, 'Jesse', 'Physics', 15, 70000.00),
(7, 'Hank', 'Chemistry', 9, 53000.00),
(8, 'Anna Red', 'Biology', 7, 51000.00);
-- Create AFTER INSERT trigger to log new inserts
CREATE TABLE IF NOT EXISTS teacher_log (
    teacher_id INT,
    action VARCHAR(50),
    timestamp DATETIME
);

CREATE TRIGGER after_insert_teacher
AFTER INSERT ON teachers
FOR EACH ROW
BEGIN
    INSERT INTO teacher_log (teacher_id, action, timestamp)
    VALUES (NEW.id, 'INSERT', datetime('now'));
END;

CREATE TRIGGER before_delete_teacher
BEFORE DELETE ON teachers
FOR EACH ROW
WHEN OLD.experience > 10
BEGIN
    SELECT RAISE(FAIL, 'Cannot delete a teacher with more than 10 years of experience');
END;

CREATE TRIGGER after_delete_teacher
AFTER DELETE ON teachers
FOR EACH ROW
BEGIN
    INSERT INTO teacher_log (teacher_id, action, timestamp)
    VALUES (OLD.id, 'DELETE', datetime('now'));
END;
