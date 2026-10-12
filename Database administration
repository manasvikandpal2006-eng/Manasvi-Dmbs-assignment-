-- MySQL 8.0+ administrative examples from the practical PDF.
-- Run with an account that has the necessary administrative privileges.
-- Use strong, unique passwords and do not publish real credentials.

-- Create users
CREATE USER 'clerk'@'localhost' IDENTIFIED BY 'ReplaceWithStrongPassword#1';
CREATE USER 'analyst'@'%' IDENTIFIED BY 'ReplaceWithStrongPassword#2';

-- Inspect users (requires permission)
SELECT User, Host FROM mysql.user;

-- Change a user's password
ALTER USER 'clerk'@'localhost' IDENTIFIED BY 'ReplaceWithAnotherStrongPassword#3';

-- Create roles and grant privileges
CREATE ROLE 'reader', 'data_entry', 'course_admin';
GRANT SELECT ON college.* TO 'reader';
GRANT SELECT, INSERT, UPDATE ON college.* TO 'data_entry';
GRANT ALL PRIVILEGES ON college.* TO 'course_admin';

-- Column-level permission example
GRANT SELECT (Rollno, Name) ON college.STUDENT TO 'reader';

-- Assign and activate a role
GRANT 'data_entry' TO 'clerk'@'localhost';
SET DEFAULT ROLE ALL TO 'clerk'@'localhost';
SHOW GRANTS FOR 'clerk'@'localhost' USING 'data_entry';

-- Revoke privileges / roles (examples; review before running)
-- REVOKE UPDATE ON college.* FROM 'data_entry';
-- REVOKE 'data_entry' FROM 'clerk'@'localhost';
-- REVOKE ALL PRIVILEGES, GRANT OPTION FROM 'course_admin';
-- DROP ROLE IF EXISTS 'course_admin';

-- Index examples
CREATE INDEX idx_student_name ON college.STUDENT(Name);
CREATE INDEX idx_course_type ON college.COURSE(Coursetype);
CREATE INDEX idx_adm_sid_date ON college.ADMISSION(SID, Dateofadmission);

-- Cname already has a UNIQUE constraint, which creates a unique index automatically.
-- Do not create a duplicate unique index for Cname.

SHOW INDEX FROM college.STUDENT;
EXPLAIN SELECT * FROM college.STUDENT WHERE Name = 'Ankit Sharma';

-- Example cleanup (only if you intentionally want to remove the index)
-- DROP INDEX idx_student_name ON college.STUDENT;

-- Optional cleanup; uncomment only if intended:
-- DROP USER IF EXISTS 'analyst'@'%';
