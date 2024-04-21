# Nama : Hanna Sajidah
# NIM : 121450060
# Kelas : RC

# Contoh Materialized Views:

# 1. Materialized view untuk menampilkan total kapasitas ruangan per gedung:
CREATE MATERIALIZED VIEW mv_total_capacity_per_building AS
SELECT building, SUM(capacity) AS total_capacity
FROM classroom
GROUP BY building;

# 2. Materialized view untuk menampilkan total anggaran departemen per gedung:
CREATE MATERIALIZED VIEW mv_total_budget_per_building AS
SELECT d.building, SUM(d.budget) AS total_budget
FROM department d
GROUP BY d.building;

# 3. Materialized view untuk menampilkan rata-rata gaji instruktur per departemen:
CREATE MATERIALIZED VIEW mv_avg_salary_per_department AS
SELECT i.dept_name, AVG(i.salary) AS avg_salary
FROM instructor i
GROUP BY i.dept_name;

# 4. Materialized view untuk menyimpan hasil join antara tabel mahasiswa dan mata kuliah yang diambil:
CREATE MATERIALIZED VIEW mv_student_course_join AS
SELECT s.ID, s.name, t.course_id, t.grade
FROM student s
JOIN takes t ON s.ID = t.ID;

# 5. Materialized view untuk menampilkan total kredit yang diambil oleh setiap mahasiswa:
CREATE MATERIALIZED VIEW mv_total_credits_per_student AS
SELECT ID, SUM(tot_cred) AS total_credits
FROM student
GROUP BY ID;

# Contoh Transaksi:
# 1. Transaksi untuk menambahkan departemen baru ke dalam database:
BEGIN TRANSACTION;
INSERT INTO department (dept_name, building, budget) 
VALUES ('New Department', 'New Building', 100000.00);
COMMIT;

# 2. Transaksi untuk mengupdate gaji seorang instruktur:
BEGIN TRANSACTION;
UPDATE instructor 
SET salary = 50000.00
WHERE ID = '12345';
COMMIT;

# 3. Transaksi untuk menghapus mata kuliah yang tidak diinginkan dari sebuah departemen:
BEGIN TRANSACTION;
DELETE FROM course 
WHERE dept_name = 'Computer Science' AND credits = 3;
COMMIT;

# 4. Transaksi untuk menambahkan mahasiswa baru ke dalam database:
BEGIN TRANSACTION;
INSERT INTO student (ID, name, dept_name, tot_cred) 
VALUES ('54321', 'New Student', 'Mathematics', 0);
COMMIT;

# 5. Transaksi untuk mengupdate nilai seorang mahasiswa pada suatu mata kuliah:
BEGIN TRANSACTION;
UPDATE takes 
SET grade = 'A+'
WHERE ID = '54321' AND course_id = 'CS101' AND semester = 'Fall' AND year = 2023;
COMMIT;
