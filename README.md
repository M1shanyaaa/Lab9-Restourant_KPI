# Lab9 - Restourant_KPI (Service)

**Коротко:**
Проєкт — Spring Boot (Maven) веб‑сервіс для простого ресторанного додатку: логін/адмінка, меню та ціноутворення, оформлення та обробка замовлень. Пакування — WAR. Шаблони фронтенду — Thymeleaf у `src/main/resources/templates`.

---

## Швидкий старт

### Вимоги

* Java 17 або новіша (рекомендовано JDK 17+)
* Maven 3.6+
* MySQL (або інша СУБД, сумісна з Spring Data JPA)

### Налаштування БД

За замовчуванням у `src/main/resources/application.properties`:

```
server.port=8081
spring.application.name=service

spring.jpa.hibernate.ddl-auto=update
spring.datasource.url=jdbc:mysql://localhost:3306/spring_wed_service
spring.datasource.username=root
spring.datasource.password=root
```

Створіть базу та користувача (приклад для MySQL):

```sql
CREATE DATABASE spring_wed_service CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'root'@'localhost' IDENTIFIED BY 'root';
GRANT ALL PRIVILEGES ON spring_wed_service.* TO 'root'@'localhost';
FLUSH PRIVILEGES;
```

> Якщо ви працюєте з іншими даними доступу — змініть `spring.datasource.*` у `application.properties`.

### Збірка та запуск

У корені проєкту виконайте:

```bash
# збірка артефакту
mvn clean package

# або запустити прямо через Maven
mvn spring-boot:run
```

Після запуску додаток буде доступний за адресою `http://localhost:8081/` (порт можна змінити в properties).

> Якщо після `mvn package` у папці `target` з'явився файл `service-0.0.1-SNAPSHOT.war`, можна запустити:
>
> ```bash
> java -jar target/service-0.0.1-SNAPSHOT.war
> ```

---

## Структура проєкту (ключові каталоги)

```
├─ src/main/java/com/resrourant/service
│  ├─ controllers          # MVC контролери (AdminController, LoginController, MainController, MenuAndPricingController, OrdersController)
│  ├─ models               # JPA-моделі: MenuItem, Order, Login, etc.
│  ├─ repo                 # Spring Data repositories
│  └─ service              # (можливо) бізнес-логіка
├─ src/main/resources
│  ├─ templates           # Thymeleaf-шаблони (about.html, home.html, admin.html, login.html, menupricing.html, order.html, ...)
│  └─ application.properties
├─ pom.xml                # Maven + залежності Spring Boot
```

## Основні функції / ендпойнти

(Контролери реалізовані як MVC з Thymeleaf):

* `GET /` — головна сторінка (home)
* `GET /about` — сторінка "Про проект"
* `GET, POST /login` — форма логіну та обробка
* `GET /admin` — доступ до панелі адміністратора
* `GET, POST /menupricing` — перегляд/додавання пунктів меню та ціноутворення
* `GET /order` — сторінка оформлення замовлення, список доступних позицій
* `POST /order/delete` — видалення замовлення (реалізація у OrdersController)
* `GET/POST /manipulateorder` — адміністративні дії з замовленнями (змінити стан готовності тощо)

> Для повного переліку методів — подивіться файли у `src/main/java/com/resrourant/service/controllers/`.

---

## Нотатки по базі даних

* `spring.jpa.hibernate.ddl-auto=update` — Hibernate автоматично створює/оновлює таблиці відповідно до JPA-ентиті.
* Якщо ви хочете мати контроль над схемою — змініть налаштування на `validate` і створіть SQL-скрипти вручну.

---

## Відомі моменти / рекомендації

* Порт сервера налаштовано на `8081`. Якщо на вашій машині порт зайнятий — змініть його у `application.properties`.
* Пакування — **WAR**. Spring Boot дозволяє запускати WAR як fat‑jar, але якщо плануєте деплой у зовнішній Tomcat — перевірте конфігурацію `ServletInitializer` (якщо він є).
* Якщо ви змінюєте модель — додавайте `@Transactional` у відповідних місцях та перевіряйте міграції даних.

---


**PR опис:**

* Що зроблено
* Чому
* Як це тестувати

---

## Тестування

У проєкті є тестовий клас `src/test/java/com/resrourant/service/ServiceApplicationTests.java`. Запуск тестів:

```bash
mvn test
```

---

© Lab9 — Restourant_KPI
