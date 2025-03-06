# db-university

<!-- EX - Query con SELECT -->

1. Selezionare tutti gli studenti nati nel 1990 (160)
   SELECT \* FROM `students` WHERE YEAR(`date_of_birth`) = 1990

2. Selezionare tutti i corsi che valgono più di 10 crediti (479)
   SELECT \* FROM `courses` WHERE `cfu` > 10

3. Selezionare tutti gli studenti che hanno più di 30 anni
   SELECT \* FROM `students` WHERE DATE(`date_of_birth`) < '1995-03-05'

4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di laurea (286)
   SELECT \* from `courses` WHERE `year` = 1 AND `period` = "I semestre"

5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del 20/06/2020 (21)
   SELECT \* from `exams` WHERE TIME(`hour`) > '14:00' AND DATE(`date`) = '2020-06-20'

6. Selezionare tutti i corsi di laurea magistrale (38)
   SELECT \* from `degrees` WHERE `level` = 'magistrale'

7. Da quanti dipartimenti è composta l'università? (12)
   SELECT COUNT(id) from `departments`

<!-- EX - Query con GROUP BY -->

1. Contare quanti iscritti ci sono stati ogni anno
   SELECT COUNT(id), YEAR(`enrolment_date`) from `students` GROUP BY YEAR(`enrolment_date`)

2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio
   SELECT COUNT(id), `office_address` FROM `teachers` GROUP BY `office_address`

3. Calcolare la media dei voti di ogni appello d'esame
   SELECT AVG(`vote`), `exam_id` FROM `exam_student` GROUP BY `exam_id`

4. Contare quanti corsi di laurea ci sono per ogni dipartimento
   SELECT COUNT(id), `department_id` FROM `degrees` GROUP BY `department_id`

<!-- EX - Query con JOIN -->

1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia
   SELECT `students`.id, `students`.name, `students`.surname, `degrees`.name
   FROM `students`
   JOIN `degrees`
   ON `degree_id` = `degrees`.id
   WHERE `degrees`.name = 'Corso di Laurea in Economia'

2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze
   SELECT `departments`.id, `departments`.name, `degrees`.name, `degrees`.level
   FROM `degrees`
   JOIN `departments`
   ON `department_id` = `departments`.id
   WHERE `departments`.name = 'Dipartimento di Neuroscienze'
   AND `degrees`.level = 'magistrale'

3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
   SELECT `courses`.\*
   FROM `courses`
   JOIN `course_teacher` ON `courses`.id = `course_teacher`.course_id
   JOIN `teachers` ON `teachers`.id = `course_teacher`.teacher_id
   WHERE `teachers`.name = 'Fulvio' AND `teachers`.surname = 'Amato'

4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui
   sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome
   SELECT `students`.id, `students`.name, `students`.surname, `degrees`.name, `degrees`.level, `departments`.name
   FROM `students`
   JOIN `degrees` ON `degrees`.id = `students`.degree_id
   JOIN `departments` ON `departments`.id = `degrees`.department_id
   ORDER BY `students`.surname, `students`.name

5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti
   SELECT `degrees`.name, `courses`.name, `courses`.description, `courses`.period, `courses`.cfu, `teachers`.name, `teachers`.surname
   FROM `degrees`
   JOIN `courses` ON `degrees`.id = `courses`.degree_id
   JOIN `course_teacher` ON `courses`.id = `course_teacher`.course_id
   JOIN `teachers` ON `teachers`.id = `course_teacher`.teacher_id

6. Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)
   SELECT `teachers`.\*, `departments`.name
   FROM `departments`
   JOIN `degrees` ON `departments`.id = `degrees`.department_id
   JOIN `courses` ON `degrees`.id = `courses`.degree_id
   JOIN `course_teacher` ON `courses`.id = `course_teacher`.course_id
   JOIN `teachers` ON `teachers`.id = `course_teacher`.teacher_id
   WHERE `departments`.name = 'Dipartimento di Matematica'
   GROUP BY `teachers`.id

7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti
   per ogni esame, stampando anche il voto massimo. Successivamente,
   filtrare i tentativi con voto minimo 18.
