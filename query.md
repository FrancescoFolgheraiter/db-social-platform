1. Seleziona tutti gli utenti e calcolane l'età (25)

SELECT username, 
    YEAR(CURRENT_DATE) - YEAR(birthdate) - 
    (CASE 
        WHEN MONTH(CURRENT_DATE) < MONTH(birthdate) OR 
             (MONTH(CURRENT_DATE) = MONTH(birthdate) AND 
              DAY(CURRENT_DATE) < DAY(birthdate)) 
        THEN 1 
        ELSE 0 
    END) AS accurate_age
FROM users;


__________________________________________________________

2. Seleziona tutti i post senza Like (13)


SELECT posts.id, posts.title, posts.user_id
FROM posts
WHERE posts.id NOT IN (
	SELECT likes.post_id
    FROM likes
    WHERE 1
    GROUP BY likes.post_id
);

___________________________________________________________

3. Conta il numero di like per ogni post (165)

SELECT COUNT(*) AS `Total_likes`, post_id 
FROM `likes` 
GROUP BY post_id;


___________________________________________________________

4. Ordina gli utenti per il numero di media caricati (25)

SELECT COUNT(*) AS Total_media, users.username 
FROM `medias`
INNER JOIN users
ON medias.user_id = users.id
GROUP BY user_id
ORDER BY Total_media DESC;


___________________________________________________________

5. Ordina gli utenti per totale di likes ricevuti nei loro posts (25)

SELECT COUNT(*) as total_likes, users.username
FROM likes
INNER JOIN users
ON users.id = likes.user_id
GROUP BY user_id
ORDER BY total_likes DESC;


___________________________________________________________

6. Seleziona tutti i post degli utenti tra i 20 e i 30 anni (49)

SELECT posts.id, posts.title, users.username, TIMESTAMPDIFF(YEAR, users.birthdate, CURDATE()) as Age
FROM posts
INNER JOIN users
ON users.id = posts.user_id
WHERE TIMESTAMPDIFF(YEAR, users.birthdate, CURDATE()) BETWEEN 20 AND 30;

//chiedere riguardo non ho la possibilità di richiamare age
//per il motivo che age non esiste


___________________________________________________________

7. Seleziona il numero di post e di media per ogni utente (25)

SELECT users.username, COUNT(DISTINCT posts.id) AS total_post, COUNT(DISTINCT medias.id) AS total_media
FROM `users`
INNER JOIN posts
ON posts.user_id = users.id
INNER JOIN medias
ON medias.user_id = users.id
GROUP BY users.id;