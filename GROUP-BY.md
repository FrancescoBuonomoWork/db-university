1. Contare quanti iscritti ci sono stati ogni anno

- SELECT COUNT(*) AS `students_number`,YEAR(`enrolment_date`) AS `year` FROM `students` GROUP BY `year`;


2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio

- SELECT COUNT(*) AS `teachers_number`,`office_address` FROM `teachers` GROUP BY `office_address`;


3. Calcolare la media dei voti di ogni appello d'esame

- SELECT COUNT(*) AS `exams_number`,AVG(`vote`) AS `average` FROM `exam_student` GROUP BY `exam_id`;


4. Contare quanti corsi di laurea ci sono per ogni dipartimento

- SELECT COUNT(*),`department_id` AS `courses_by_departments` FROM `degrees` GROUP BY `department_id`;