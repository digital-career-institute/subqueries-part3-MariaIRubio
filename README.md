# Table 1: JobOpenings
```SQL
 CREATE TABLE JobOpenings (
         JobID INT PRIMARY KEY,
         JobTitle VARCHAR(255),
         Department VARCHAR(255),
         MinimumExperience INT,
         MinimumEducation VARCHAR(255)
);
 ```
# Table 2: Applicants
```SQL
CREATE TABLE Applicants (
	ApplicantID INT PRIMARY KEY,
	ApplicantName VARCHAR(255),
	ExperienceYears INT,
	Education VARCHAR(255),
	AppliedForJobID INT,
	FOREIGN KEY(AppliedForJobID) REFERENCES JobOpenings(JobID)
);
 ```

![tablestructure](https://github.com/NootanVijapure/subqueries_part3/assets/30225165/72f6ba1d-dbca-4bc6-aa11-f34cbbf14fb3)

# data entry sample data
```SQL
INSERT INTO JobOpenings VALUES 
	(101, 'Spacecraft Pilot', 'Flight Operations', 5, 'Bachelors in Aerospace Engineering'),
	(102, 'Mission Specialist', 'Science', 3, 'PhD in Physics or related field'),
	(103, 'Space Engineer', 'Engineering', 3, 'Bachelors in Mechanical Engineering'),
	(104, 'Communications Officer', 'Communication', 2, 'Bachelors in Communication');
	
UPDATE JobOpenings SET MinimumEducation = 'PhD in Physics' WHERE JobID = 102;
 ```

```SQL
INSERT INTO Applicants VALUES
	(501, 'John Astronaut', 7, 'Masters in Aerospace Engineering', 101),
	(502, 'Lisa Scientist', 4, 'PhD in Physics', 102),
	(503, 'Mark Engineer', 5, 'Bachelors in Mechanical Engineering', 103),
	(504, 'Emily COmmunicator', 3, 'Bachelors in Communication', 104);

INSERT INTO Applicants VALUES
	(505, 'Maria Ibanez', 1, 'Software Developer', null);
 ```

![dataentrytable1](https://github.com/NootanVijapure/subqueries_part3/assets/30225165/c91699b4-e1ba-4b55-bd64-f4ac7dbd3417)
![dataentrytable2](https://github.com/NootanVijapure/subqueries_part3/assets/30225165/c6250e16-afa8-402e-81ce-36e5d2c1e626)


## 1.List All Job Openings:
 Retrieve a list of all available job openings, including details about the title, department, minimum experience, and minimum education.
```SQL
 SELECT * FROM JobOpenings;
 ```
## 2. Applicants and Their Jobs:
Display the names of applicants and the jobs they have applied for. Include all applicants, even if they haven't applied for any job.
```SQL
SELECT 
 a.ApplicantName, 
 a.AppliedForJobID, 
 j.JobTitle, 
 j.Department 
FROM 
 Applicants a
LEFT JOIN 
 JobOpenings j ON j.JobID = a.AppliedForJobID;
 ```
## 3. Qualified Applicants:
Create a report showing the names of applicants who meet or exceed the minimum experience and education requirements for the jobs they have applied for.
```SQL
SELECT 
 a.ApplicantName, 
 a.Education, 
 a.ExperienceYears, 
 j.JobTitle, 
 j.MinimumEducation, 
 j.MinimumExperience
FROM 
 Applicants a
JOIN 
 JobOpenings j ON j.JobID = a.AppliedForJobID
WHERE 
 a.ExperienceYears >= j.MinimumExperience
AND 
 a.Education >=  j.MinimumEducation;
 ```
## 4. Job Applicants Without Match:
Identify applicants who have applied for a job that currently has no opening.
```SQL
SELECT 
 a.ApplicantName 
FROM 
 Applicants a
WHERE 
 AppliedForJobID NOT IN (SELECT j.JobID FROM JobOpenings j);
 ```
## 5.Experience Range Search:
Find all job openings that require a minimum of 3 to 5 years of experience.
```SQL
SELECT 
 JobTitle, 
 MinimumExperience
FROM 
 JobOpenings
WHERE 
 MinimuMExperience BETWEEN 3 AND 5;
```
## 6. Education Level Analysis:
Retrieve a list of job titles and the count of applicants who have applied for each job, grouped by education level (e.g., Bachelor's, Master's, PhD).
```SQL
SELECT 
 j.JobTitle, 
 j.MinimumEducation, 
 COUNT(a.AppliedForJobID) AS NumberApplicants
FROM 
 JobOpenings j
JOIN 
 Applicants a ON j.JobID = a.AppliedForJobID
GROUP BY 
 j.JobTitle, j.MinimumEducation
ORDER BY 
 j.MinimumEducation DESC;
```
