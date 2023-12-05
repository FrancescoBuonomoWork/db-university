<!-- PS  lascio dei commenti dove sono i ragionamenti che ho fatto prima di arrivare alla soluzione finale 
 -->
1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia

- SELECT `degrees`.`name`, `students`. * FROM `degrees` INNER JOIN `students` ON `degrees`.`id` = `students`.`degree_id` WHERE `degrees`.`name` = 'Corso di Laurea in Economia';


2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di
Neuroscienze

- SELECT * FROM `departments` INNER JOIN `degrees` ON `departments`.`id` = `degrees`.`department_id` WHERE `departments`.`name` = 'Dipartimento di Neuroscienze' AND `degrees`.`level` = 'magistrale';


3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)


- SELECT * FROM `teachers` INNER JOIN `course_teacher` ON `teachers`.`id` = `course_teacher`.`teacher_id` INNER JOIN `courses` ON `course_teacher`.`course_id`=`courses`.`id` WHERE `teachers`.`id` = 44;


4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e
nome

<!-- - SELECT * FROM `students`
INNER JOIN `degrees` ON `students`.`degree_id`= `degrees`.`id`
INNER JOIN `departments`on `departments`.`id` = `degrees`.`department_id`;

- SELECT `students`.*,`degrees`.`name`,`departments`.`name` FROM `students` INNER JOIN `degrees` ON `students`.`degree_id`= `degrees`.`id` INNER JOIN `departments`on `departments`.`id` = `degrees`.`department_id`; -->

<!-- - SELECT CONCAT(`students`.`surname`, ' ',`students`.`name`),`degrees`.`name`,`departments`.`name` FROM `students` INNER JOIN `degrees` ON `students`.`degree_id`= `degrees`.`id` INNER JOIN `departments`on `departments`.`id` = `degrees`.`department_id`; -->

- SELECT CONCAT(`students`.`surname`, ' ',`students`.`name`) AS `full_name`,`degrees`.`name`,`departments`.`name` FROM `students` INNER JOIN `degrees` ON `students`.`degree_id`= `degrees`.`id` INNER JOIN `departments`on `departments`.`id` = `degrees`.`department_id` ORDER BY `full_name` ASC;


5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti

<!-- - SELECT * FROM `degrees` INNER JOIN `courses` ON `degrees`.`id` = `courses`.`degree_id` INNER JOIN `course_teacher` ON `courses`.`id` = `course_teacher`.`course_id` INNER JOIN `teachers` ON `course_teacher`.`teacher_id`= `teachers`.`id`; -->
- SELECT `degrees`.`name`,`courses`.`name`,`teachers`.* FROM `degrees` INNER JOIN `courses` ON `degrees`.`id` = `courses`.`degree_id` INNER JOIN `course_teacher` ON `courses`.`id` = `course_teacher`.`course_id` INNER JOIN `teachers` ON `course_teacher`.`teacher_id`= `teachers`.`id`;

6. Selezionare tutti i docenti che insegnano nel Dipartimento di
Matematica (54)

- SELECT `departments`.`name`,`teachers`.* FROM `departments` INNER JOIN `degrees` ON `departments`.`id`=`degrees`.`department_id` INNER JOIN `courses` ON `degrees`.`id` = `courses`.`degree_id` INNER JOIN `course_teacher` ON `courses`.`id` = `course_teacher`.`course_id` INNER JOIN `teachers` ON `course_teacher`.`teacher_id`= `teachers`.`id` WHERE `departments`.`name` = 'Dipartimento di Matematica';


7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti per ogni esame, stampando anche il voto massimo. Successivamente,
filtrare i tentativi con voto minimo 18.

<!-- INNER JOIN `students`ON `students`.`id`= `exam_student`.`student_id`; -->
SELECT *
FROM (
    SELECT
    	`stu`.`id` as studId,
    	`stu`.`name` as studName,
    	`stu`.`surname` as studSurname,
    	`cou`.`id` as courseId,
    	`cou`.`name` as corsoName,
    	COUNT(*) as numTentativi,
    	MAX(`ex_stu`.`vote`) as votoMax
FROM `students` as `stu`
  JOIN `exam_student` as `ex_stu`
  	ON `stu`.`id` = `ex_stu`.`student_id`
  JOIN `exams` as `ex`
    ON `ex_stu`.`exam_id` = `ex`.`id`
  JOIN `courses` as `cou`
  	ON `ex`.`course_id` = `cou`.`id`
GROUP BY `stu`.`id`, `stu`.`name`, `stu`.`surname`, `cou`.`id`, `cou`.`name`) as tmp
WHERE `tmp`.`votoMax` >= 18;

SELECT *
FROM (
    SELECT
    	`stu`.`id` as studId,
    	`stu`.`name` as studName,
    	`stu`.`surname` as studSurname,
    	`cou`.`id` as courseId,
    	`cou`.`name` as corsoName,
    	COUNT(*) as numTentativi,
    	MAX(`ex_stu`.`vote`) as votoMax
    FROM `students` as `stu`
        JOIN `exam_student` as `ex_stu`
  	        ON `stu`.`id` = `ex_stu`.`student_id`
        JOIN `exams` as `ex`
            ON `ex_stu`.`exam_id` = `ex`.`id`
        JOIN `courses` as `cou`
  	        ON `ex`.`course_id` = `cou`.`id`
    GROUP BY `stu`.`id`, `stu`.`name`, `stu`.`surname`, `cou`.`id`, `cou`.`name`
    ) as tmp
WHERE `tmp`.`votoMax` >= 18;

    SELECT
    	`stu`.`id` as studId,
    	`stu`.`name` as studName,
    	`stu`.`surname` as studSurname,
    	`cou`.`id` as courseId,
    	`cou`.`name` as corsoName,
    	COUNT(*) as numTentativi,
    	MAX(`ex_stu`.`vote`) as votoMax
    FROM `students` as `stu`
        JOIN `exam_student` as `ex_stu`
  	        ON `stu`.`id` = `ex_stu`.`student_id`
        JOIN `exams` as `ex`
            ON `ex_stu`.`exam_id` = `ex`.`id`
        JOIN `courses` as `cou`
  	        ON `ex`.`course_id` = `cou`.`id`
    GROUP BY `stu`.`id`, `stu`.`name`, `stu`.`surname`, `cou`.`id`, `cou`.`name`
    HAVING `votoMax` >= 18;