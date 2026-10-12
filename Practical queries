USE college;

-- Q1. Names of students enrolled in any course
SELECT DISTINCT s.Name
FROM STUDENT s
JOIN ADMISSION a ON s.Rollno = a.Rollno;

-- Q2. Students enrolled in at least one part-time course
SELECT DISTINCT s.Name
FROM STUDENT s
JOIN ADMISSION a ON s.Rollno = a.Rollno
JOIN COURSE c ON a.SID = c.SID
WHERE c.Coursetype = 'Parttime';

-- Q3. Student names starting with A
SELECT Name FROM STUDENT WHERE Name LIKE 'A%';

-- Q4. Student details studying Computer Science or Chemistry
SELECT DISTINCT s.Rollno, s.Name, s.Dateofbirth
FROM STUDENT s
JOIN ADMISSION a ON s.Rollno = a.Rollno
JOIN COURSE c ON a.SID = c.SID
WHERE c.Cname IN ('Computer Science', 'Chemistry');

-- Q5. Roll number starts with X or Z and ends with 9
SELECT Name
FROM STUDENT
WHERE (Rollno LIKE 'X%' OR Rollno LIKE 'Z%')
  AND Rollno LIKE '%9';

-- Q6. Course details with more than N students; change @N as needed
SET @N = 3;
SELECT c.SID, c.Cname, c.TotalSeats, c.Duration, c.Coursetype,
       c.TeacherInCharge, COUNT(a.Rollno) AS Enrolled
FROM COURSE c
JOIN ADMISSION a ON c.SID = a.SID
GROUP BY c.SID, c.Cname, c.TotalSeats, c.Duration, c.Coursetype, c.TeacherInCharge
HAVING COUNT(a.Rollno) > @N;

-- Q7. Update a student's name (example target from PDF)
UPDATE STUDENT
SET Name = 'Ankit Kumar Sharma'
WHERE Rollno = 'X2001';

-- Q8. Course names with more than five admissions
SELECT c.Cname, COUNT(*) AS Enrolled
FROM ADMISSION a
JOIN COURSE c ON a.SID = c.SID
GROUP BY c.SID, c.Cname
HAVING COUNT(*) > 5;

-- Q9. Youngest student(s) enrolled in BSc(P)CS; includes ties
SELECT s.Name, s.Dateofbirth
FROM STUDENT s
JOIN ADMISSION a ON s.Rollno = a.Rollno
JOIN COURSE c ON a.SID = c.SID
WHERE c.Cname = 'BSc(P)CS'
  AND s.Dateofbirth = (
    SELECT MAX(s2.Dateofbirth)
    FROM STUDENT s2
    JOIN ADMISSION a2 ON s2.Rollno = a2.Rollno
    JOIN COURSE c2 ON a2.SID = c2.SID
    WHERE c2.Cname = 'BSc(P)CS'
  );

-- Q10. Most popular society/societies by member count; includes ties
SELECT y.Socname, COUNT(*) AS Members
FROM SOCIETY y
JOIN ENROLLMENT e ON y.SocID = e.SocID
GROUP BY y.SocID, y.Socname
HAVING COUNT(*) = (
  SELECT MAX(cnt)
  FROM (SELECT COUNT(*) AS cnt FROM ENROLLMENT GROUP BY SocID) AS t
);

-- Q11. Two most popular part-time courses
SELECT c.Cname, COUNT(*) AS Enrolled
FROM COURSE c
JOIN ADMISSION a ON c.SID = a.SID
WHERE c.Coursetype = 'Parttime'
GROUP BY c.SID, c.Cname
ORDER BY Enrolled DESC, c.Cname
LIMIT 2;

-- Q12. Students admitted to full-time courses only; excludes students with no admission
SELECT DISTINCT s.Rollno, s.Name
FROM STUDENT s
JOIN ADMISSION a ON s.Rollno = a.Rollno
WHERE NOT EXISTS (
  SELECT 1
  FROM ADMISSION a2
  JOIN COURSE c2 ON a2.SID = c2.SID
  WHERE a2.Rollno = s.Rollno
    AND c2.Coursetype <> 'Fulltime'
);

-- Q13. Course names with more than 30 admissions
SELECT c.Cname, COUNT(*) AS Admitted
FROM ADMISSION a
JOIN COURSE c ON a.SID = c.SID
GROUP BY c.SID, c.Cname
HAVING COUNT(*) > 30;

-- Q14. Students with admissions and courses with at least one student
SELECT DISTINCT s.Name AS Item, 'Student' AS Kind
FROM STUDENT s
JOIN ADMISSION a ON s.Rollno = a.Rollno
UNION
SELECT DISTINCT c.Cname, 'Course'
FROM COURSE c
JOIN ADMISSION a ON c.SID = a.SID;

-- Q15. Full-time courses where teacher's name contains Gupta
SELECT Cname, TeacherInCharge
FROM COURSE
WHERE TeacherInCharge LIKE '%Gupta%'
  AND Coursetype = 'Fulltime';

-- Q16. Courses whose enrolment is exactly 10% of total seats
SELECT c.Cname, c.TotalSeats, COUNT(a.Rollno) AS Enrolled
FROM COURSE c
LEFT JOIN ADMISSION a ON c.SID = a.SID
GROUP BY c.SID, c.Cname, c.TotalSeats
HAVING COUNT(a.Rollno) = c.TotalSeats * 0.10;

-- Q17. Vacant seats for each course
SELECT c.Cname, c.TotalSeats, COUNT(a.Rollno) AS Filled,
       c.TotalSeats - COUNT(a.Rollno) AS Vacant
FROM COURSE c
LEFT JOIN ADMISSION a ON c.SID = a.SID
GROUP BY c.SID, c.Cname, c.TotalSeats
ORDER BY Vacant DESC;

-- Q18. Increase every course's seats by 10%, rounding up
UPDATE COURSE SET TotalSeats = CEIL(TotalSeats * 1.10);

-- Q19. Add fees-paid flag to ENROLLMENT
ALTER TABLE ENROLLMENT
  ADD COLUMN FeesPaid VARCHAR(3) NOT NULL DEFAULT 'No'
  CHECK (FeesPaid IN ('Yes', 'No'));

-- Q20. Move all admission dates forward by one year
UPDATE ADMISSION
SET Dateofadmission = DATE_ADD(Dateofadmission, INTERVAL 1 YEAR);

-- Q21. View of course enrolment and vacancies
CREATE OR REPLACE VIEW CourseEnrolment AS
SELECT c.SID, c.Cname, c.TotalSeats, COUNT(a.Rollno) AS TotalEnrolled,
       c.TotalSeats - COUNT(a.Rollno) AS Vacant
FROM COURSE c
LEFT JOIN ADMISSION a ON c.SID = a.SID
GROUP BY c.SID, c.Cname, c.TotalSeats;

SELECT * FROM CourseEnrolment ORDER BY TotalEnrolled DESC;

-- Q22. Number of courses with more than five students, by course type
SELECT Coursetype, COUNT(*) AS PopularCourses
FROM (
  SELECT c.SID, c.Coursetype
  FROM COURSE c
  JOIN ADMISSION a ON c.SID = a.SID
  GROUP BY c.SID, c.Coursetype
  HAVING COUNT(*) > 5
) AS popular
GROUP BY Coursetype;

-- Q23. Add Mobile column (default shown in practical PDF)
ALTER TABLE STUDENT
  ADD COLUMN Mobile CHAR(10) DEFAULT '9999999999';

-- Q24. Number of students older than 18 (completed years)
SELECT COUNT(*) AS StudentsOver18
FROM STUDENT
WHERE TIMESTAMPDIFF(YEAR, Dateofbirth, CURDATE()) > 18;

-- Q25. Students born in 2001 admitted to at least one part-time course
SELECT DISTINCT s.Name, s.Dateofbirth
FROM STUDENT s
JOIN ADMISSION a ON s.Rollno = a.Rollno
JOIN COURSE c ON a.SID = c.SID
WHERE s.Dateofbirth >= '2001-01-01'
  AND s.Dateofbirth < '2002-01-01'
  AND c.Coursetype = 'Parttime';
