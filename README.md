
# db-university

QUERY SELECT

1. Selezionare tutti gli studenti nati nel 1990 (160)
2. Selezionare tutti i corsi che valgono più di 10 crediti (479)
3. Selezionare tutti gli studenti che hanno più di 30 anni
4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di laurea (286)
5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del 20/06/2020 (21)
6. Selezionare tutti i corsi di laurea magistrale (38)
7. Da quanti dipartimenti è composta l'università? (12)
8. Quanti sono gli insegnanti che non hanno un numero di telefono? (50)


1. 
SELECT `name`,`surname`,`date_ofbirth`
FROM `student`
WHERE YEAR (`date_of_birth`) = 1990

2. 
SELECT *
FROM `courses`
WHERE `cfu` > 10

3. 
SELECT *
FROM `students`
WHERE TIMESTAPDIFF(YEAR,`date_of_birth`,CURDATE()) >30 ORDER by `students`.`date_of_birth` DESC

4. 

SESLECT *
FROM `courses`
WHERE `period`= "I semetre"
AND `year`= 1

5. 
SESLECT * 
FROM `exam`
WHERE `date`+ '2020/06-20' AND HOUR (`hour`) >= "14"ORDER BY `exam`.`hour`ASC

6. 
SELECT *
FROM `degrees`
WHERE `level`= "magistrale"

7. 

SELECT COUNT (*)
FROM `departments`

8. 
SELECT COUNT (*)
FROM `teachers`
WHERE `phone` IS NULL


 


QUERY GROUP BY

1. Contare quanti iscritti ci sono stati ogni anno
2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio
3. Calcolare la media dei voti di ogni appello d'esame
4. Contare quanti corsi di laurea ci sono per ogni dipartimento



1. 
SELECT COUNT(*), YEAR(`enrolment_date`)
FROM `students`
GROUP BY YEAR (`enrolment_date`)

2. 
SELECT COUNT(*), `office_address`
FROM `teachers`
GROUP BY `office_address`

3. 
SELECT AGV (`vote`), `exam_id`
FROM `exam_student`
GROUP BY `exam_id`

4. 
SELECT COUNT(*), `degree_id`
FROM `courses`
GGROUP BY `degree_id`



QUErY JOIN

1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia
2. Selezionare tutti i Corsi di Laurea del Dipartimento di Neuroscienze
3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome
5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti
6. Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)
7. BONUS: Selezionare per ogni studente quanti tentativi d’esame ha sostenuto per superare ciascuno dei suoi esami


1. 
SELECT `students`.`name`, `students`.`surname`, `students`.`registration_number`
FROM `students`
JOIN `degrees` ON `students`.`degree_id` = `degrees`.`id`
WHERE `degrees`.`name`= "Corso di Laurea in Economia"

2. 
SELECT `departments`.`name`, `degrees`.`name`, `degrees`.`level`
FROM `departments`
JOIN `degrees` ON `departments`.`id` = `degrees`.`id`
WHERE `departments`.`name`= "Dipartimento di Neuroscienze"

3. 
SELECT `teachers`.`id`AS "ID_insegnante", `courses`. `name`, `courses`.`description`
FROM `courses`
JOIN `course_teacher` ON `course_teacher`.`course_id` = `courses`.`id`
JOIN `teachers` ON `course_teacher`.`teacher_id` = `teachers`.`id`
WHERE `teachers`.`name`= "FULVIO" AND `teachers`. `surname` = "AMATO"
  
4. 
SELECT `students`.`name`,`students`.`surname`,`students`.`registration_number`,`degrees`.`name`,`departments`.`name`
FROM `students`
JOIN `degrees` ON `students`.`degree_id` = `degrees`.`id`
JOIN `departments` ON `degrees`.`departments_id` = `departments`.`id`
ORDER BY `students`. `surname`, `student`.`name`

5. 
SELECT `degrees`.`name`,`courses`. `name`, `teachers`.`name`, `teachers`. `surname`
FROM `degrees`
JOIN `courses` ON `degrees`.`id` = `courses`.`degree_id`
JOIN `course_teacher` ON `course`.`id` = `course_teacher`.`course_id`
JOIN `teachers`ON `teachers.id` = `course_teacher`.`teacher_id`

