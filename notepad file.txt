
-- Display all records from the hospitals table.

select * from hospitals;


-- Show all records from the doctors table.

select * from doctors;

-- Show all records from the patients table.

select * from patients;


-- List all unique Doctor_Specialization values.
select Distinct doctor_specialization from doctors;


-- Retrieve Patient_ID, Patient_Disease, and Bill_Amount from patients.
 
 select patient_id,patient_disease,bill_amount from patients
 order by  Bill_Amount desc;
 
 
 
-- Show all patients from Hyderabad city.
 select count(patient_id) as total_pa ,patient_city from patients
 group by Patient_City 
 order by total_pa desc;



-- Display Doctor_ID and Doctor_Experience for all doctors.

select Doctor_ID,doctor_experience from doctors
order by Doctor_Experience desc;


-- List all female patients.
select count(patient_id),patient_gender from patients
group by Patient_Gender ;

select count(patient_id), patient_gender from patients where Patient_Gender = 'Female';



-- Show patients whose Bill_Amount is greater than ₹10,000.

select patient_id,Bill_Amount
from patients
where Bill_Amount > 10000;


select sum(bill_amount)
from patients;


-- Display doctors with more than 5 years of Doctor_Experience.

select * 
from doctors 
where  Doctor_Experience > 5;



-- find all patients who have Insurance_Covered = 'Yes'.

select count(Patient_ID), insurance_covered from patients
where insurance_covered = 'Yes' group by Insurance_Covered ;



-- Show all distinct Patient_City values.

select distinct patient_city from patients;



-- Find all patients with Outcome = 'Recovered'.

select patient_id , outcome from patients 
where outcome = 'Recovered';


-- Count the total number of patients in the dataset.

select count(patient_id) from patients;


-- Display Patient_ID, Patient_Age, and Outcome for all patients.

select patient_id,patient_age,outcome from patients;



-- --------------MEDIUM (16–35)



-- Show each Doctor_ID and their average patient bill amount.

select doctor_id,avg(bill_amount) from patients
group by Doctor_ID ;


-- Find the number of patients treated by each Doctor_ID.
select doctor_id ,count(Patient_ID) as total_patients from patients 
group by Doctor_ID
order by total_patients desc;


-- Display the total Bill_Amount grouped by Admission_Type.
select admission_type , sum(bill_amount) AS total_bill from patients 
group by Admission_Type
order by total_bill desc ;



-- List all doctors who work in Hospital_ID = 101.
select * from doctors
where Hospital_ID =  'H0100' and Doctor_Experience > 10 ;



-- Retrieve patients whose Treatment_Duration is greater than 7 days.
select patient_id ,treatment_duration, Insurance_Covered from patients
where Treatment_Duration > 7 and Insurance_Covered = 'Yes';


-- Show the average Patient_Age grouped by Patient_Gender.

select avg(patient_age) ,patient_gender
from patients 
group by Patient_Gender;


-- Find the maximum Bill_Amount per Payment_Mode.
select max(bill_amount) ,payment_mode
from patients 
group by Payment_Mode ;



-- Display the number of patients in each Patient_City.
select count(patient_id) as total , patient_city from patients
group by Patient_City 
order by total desc;


-- Show doctors who treated patients with Outcome = 'Ongoing'.
select doctor_id , patient_id,outcome from patients
where Outcome = 'Ongoing';


-- List all patients with Follow_Up = 'Yes'.
select patient_id , follow_up from patients
where Follow_Up = 'Yes';


-- Display all patients whose Payment_Mode is 'Insurance'.
select patient_id,payment_mode from patients
where Payment_Mode = 'Insurance';


-- Show all patients whose Visit_Date falls in 2024.

select patient_id , visit_date from patients 
where Visit_Date like '%24';



-- Display the number of patients per Patient_Disease.
select count(patient_id) as count, patient_disease from patients
group by Patient_Disease 
order by count desc;


-- Show the average Treatment_Duration for each Patient_Disease.
select avg(treatment_duration) as avg ,patient_disease
from patients 
group by Patient_Disease ;


-- Find the total Bill_Amount for patients who have Insurance_Covered = 'No'.
select sum(bill_amount) as sum , insurance_covered from patients
where insurance_covered = 'No';



-- List all doctors along with the count of their patients.
select count(patient_id)  as total_pa  , doctor_id
from patients 
group by Doctor_ID
order by total_pa desc
;



-- Find doctors whose average patient bill exceeds ₹15,000.
select doctor_id , bill_amount  from patients 
where Bill_Amount > 15000;



-- Display patients whose Outcome = 'Recovered' and Follow_Up = 'No'.

select patient_id,outcome , follow_up from patients
where outcome = 'Recovered';


-- Find the most common disease among patients.

select max(patient_disease) from patients; 


-- Show all Outpatients (Admission_Type = 'Outpatient') whose Bill_Amount < 5000.

select patient_id,admission_type,bill_amount from patients 
where Admission_Type = 'outpatient' and Bill_Amount  < 5000;




-- Find the doctor who has treated the most number of patients.

select doctor_id, count(patient_id) from patients
group by Doctor_ID
order by Doctor_ID desc
limit 5;


-- Identify doctors who have no patients assigned.

SELECT d.doctor_id, d.Hospital_ID
FROM doctors d
LEFT JOIN patients p ON d.doctor_id = p.doctor_id
WHERE p.patient_id IS NULL;

-- Calculate total revenue generated per hospital (join doctors → hospitals → patients).
select h.hospital_id , h.hospital_name , sum(p.bill_amount) as total_revenue from hospitals h
join doctors d on h.Hospital_ID = d.Hospital_ID 
join patients p on d.Doctor_ID = p.Doctor_ID 
group by h.Hospital_ID , h.Hospital_Name 
order by total_revenue desc;


-- Find the top 5 patients with the highest Bill_Amount.
select patient_id , sum(bill_amount) as highest_bill from patients 
group by Patient_ID 
order by highest_bill desc
limit 5;




-- Show the average Treatment_Duration for each Outcome.
select outcome , avg(treatment_duration) as avg_durat  from patients 
group by outcome 
order by avg_durat ;




-- Display the total number of Follow_Up patients per doctor.
select doctor_id , count(follow_up) as total_follow_up from patients
group by Doctor_ID
order by total_follow_up desc;


-- Find doctors whose patients’ average Bill_Amount > ₹20,000.
select doctor_id ,avg(bill_amount) as avg_amount from patients
group by Doctor_ID 
having avg(Bill_Amount) >20000
order by avg_amount desc;

-- Identify the disease that generated the highest total billing amount.
select patient_disease , sum(bill_amount) as total_bill from patients
group by Patient_Disease
order by total_bill desc ;



-- Show the percentage of male vs female patients overall.
select patient_gender as avg , round(count(patient_id) * 100.0 / (select count(*) from patients) ,2)
from patients
group by Patient_Gender;



-- Find the oldest patient treated by each Doctor_ID.
select doctor_id,max(patient_age) from patients
group by Doctor_ID;



-- Show Doctor_ID and specialization for doctors treating patients from multiple cities.
select d.doctor_id ,d.doctor_specialization ,count(distinct p.patient_city) as cc from doctors d
join patients p on d.Doctor_ID = p.Doctor_ID 
group by d.Doctor_ID ,d.Doctor_Specialization
having COUNT(DISTINCT p.patient_city) > 1
order by cc desc;


-- Find the hospital name that has the most experienced doctor.

select h.hospital_name ,d.doctor_experience , d.doctor_id from hospitals h
join doctors d on h.Hospital_ID = d.Hospital_ID 
order by d.Doctor_Experience desc ;


-- List patients treated by doctors with Doctor_Experience > 10 years.
select p.patient_id, d.doctor_id , d.doctor_experience from
 patients p join doctors d on p.Doctor_ID = d.Doctor_ID 
 where d.Doctor_Experience > 10;



-- Rank all doctors by their total patient revenue using RANK().

SELECT 
    d.doctor_id,
    SUM(p.bill_amount) AS total_revenue,
    RANK() OVER (ORDER BY SUM(p.bill_amount) DESC) AS revenue_rank
FROM doctors d
JOIN patients p ON d.doctor_id = p.doctor_id
GROUP BY d.doctor_id
ORDER BY revenue_rank;




-- Display the total number of patients, total revenue, and average bill grouped by Outcome.
select outcome ,count(patient_id) as total_pa , sum(bill_amount) , avg(bill_amount)  as avg_am from patients 
group by outcome ;



















