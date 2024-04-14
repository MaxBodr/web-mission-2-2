-- Запрос 1: Получить список юзернеймов пользователей\
SELECT username FROM users;

-- Запрос 2: Получить количество отправленных сообщений каждым пользователем\
SELECT u.username, COUNT(m.id) AS sent_messages\
FROM users u\
JOIN messages m ON u.id = m.from\
GROUP BY u.username;

-- Запрос 3: Получить пользователя с самым большим количеством полученных сообщений и само количество\
SELECT u.username, COUNT(m.id) AS received_messages\
FROM users u\
JOIN messages m ON u.id = m.to\
GROUP BY u.username\
ORDER BY received_messages DESC\
LIMIT 1;

-- Запрос 4: Получить среднее количество сообщений, отправленное каждым пользователем\
SELECT u.username, AVG(sent_messages.count) AS average_sent_messages\
FROM (\
    SELECT from AS user_id, COUNT(*) AS count\
    FROM messages\
    GROUP BY from\
) sent_messages\
JOIN users u ON u.id = sent_messages.user_id\
GROUP BY u.username;
