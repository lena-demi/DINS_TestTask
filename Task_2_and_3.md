# Задание 2. Тест-кейсы для запроса: GET /posts?userId=<id>&title=<title>

1.	Отсутствует значение параметра title.
Шаги к исполнению: выполнить запрос GET /posts?userId=5&title= 
Ожидаемый результат: возвращается пустой список.
 
2.	Отсутствуют значения обоих параметров.
Шаги к исполнению: выполнить запрос GET /posts?userId=&title= 
Ожидаемый результат: возвращается пустой список.

3.	userId больше максимально возможного integer в js.
Шаги к исполнению: выполнить запрос 
GET /posts?userId=9007199254740992&title=sed+ab+est+est
Ожидаемый результат: возвращается пустой список.

4.	 Строковые значения параметра userId.
Шаги к исполнению: выполнить запрос GET /posts?userId=four&title=sed+ab+est+est
Ожидаемый результат: возвращается пустой список.

5.	Десятичное число в качестве userId.
Шаги к исполнению: выполнить запрос GET /posts?userId=3,2&title=sed+ab+est+est
Ожидаемый результат: возвращается пустой список.
6.	Пробел в userId.
Шаги к исполнению: выполнить запрос 
GET /posts?userId=6%20&title=sit+vel+voluptatem+et+non+libero
Ожидаемый результат: Возвращается элемент, для которого userId=6, title=”sit vel voluptatem et non libero”:
{
    "userId": 6,
    "id": 55,
    "title": "sit vel voluptatem et non libero",
    "body": "debitis excepturi ea perferendis harum libero optio\neos accusamus cum fuga ut sapiente repudiandae\net ut incidunt omnis molestiae\nnihil ut eum odit"
  }
7.	Лишний пробел в начале или конце значения title.
Шаги к исполнению: выполнить запрос 
GET /posts?userId=7&title=%20repudiandae+ea+animi+iusto%20
Ожидаемый результат: Возвращается элемент, для которого userId=7, title=” repudiandae ea animi iusto”:
{
    "userId": 7,
    "id": 66,
    "title": "repudiandae ea animi iusto",
    "body": "officia veritatis tenetur vero qui itaque\nsint non ratione\nsed et ut asperiores iusto eos molestiae nostrum\nveritatis quibusdam et nemo iusto saepe"
 }
8.	Должна быть чувствительность к регистру.
Шаги к исполнению: выполнить запрос 
GET /posts?userId=9&title=SAPIENTE+OMNIS+FUGIT+EOS
Ожидаемый результат: Возвращается пустой список.
9.	Уязвимость к XSS-атакам.
Шаги к исполнению: выполнить запрос 
GET /posts?userId=1&title=<script>alert(123)</script>
Ожидаемый результат: Возвращается пустой список.

# Задание 3. Описание бага.

Описание: При осуществлении запроса GET/posts/postId с использованием валидного (т.е. number), но несуществующего postId в качестве ответа приходит ошибка 404 (страница не найдена).

Шаги по воспроизведению:
1. В адресную строку браузера ввести запрос: https://jsonplaceholder.typicode.com/posts/101
2. Нажать enter

Ожидаемый результат: {}

Фактический результат: 404 page not found

Комментарий: Стоит добавить в код условие, что в случае получения от сервера ответа с ошибкой клиента/сервера, должен возвращаться пустой список.
