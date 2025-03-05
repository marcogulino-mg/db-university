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
