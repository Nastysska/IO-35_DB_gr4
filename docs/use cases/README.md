# Модель прецедентів 🗂️

## 📋 Загальна схема

@startuml

actor User as U
actor Manager as M
actor Administrator as A

M --|> U
A --|> M

U --> (Create User)
U --> (Authorize User)
U --> (Edit User)
U --> (Create Project)
U --> (Edit Project)
U --> (Delete Project)

M --> (Create Project)
M --> (Edit Project)
M --> (Delete Project)
M --> (Add User To Project)
M --> (Remove User From Project)
M --> (Create Board)
M --> (Delete Board)

A --> (Delete User)
A --> (Edit User)
A --> (Edit Project)
A --> (Delete Project)
A --> (Add User To Project)
A --> (Block Project)
A --> (Unblock Project)
A --> (Ban User)
A --> (Unban User)
A --> (Edit System Settings)

@enduml

## 🧑‍💻 Користувач

@startuml

actor User as U

U --> (Create User)
U --> (Authorize User)
U --> (Edit User)
U --> (Create Project)
U --> (Edit Project)
U --> (Delete Project)

@enduml

## 🛠️ Адміністратор

@startuml

actor Administrator as A

A --> (Delete User)
A --> (Edit User)
A --> (Edit Project)
A --> (Delete Project)
A --> (Add User To Project)
A --> (Block Project)
A --> (Unblock Project)
A --> (Ban User)
A --> (Unban User)
A --> (Edit System Settings)

@enduml

## 📊 Керівник

@startuml

actor Manager as M

M --> (Create Project)
M --> (Edit Project)
M --> (Delete Project)
M --> (Add User To Project)
M --> (Remove User From Project)
M --> (Create Board)
M --> (Delete Board)

@enduml

## 📝 Сценарії використання

@startuml

actor User
actor System

User -> System: Click "Register"
User -> System: Fill registration form (username, email, password)
User -> System: Click "Create"
System -> System: Validate input
System -> System: Create user account
System -> User: Automatically log in

@enduml


| **ID**           | CreateUser |
|------------------|------------|
| **НАЗВА**        | Створити користувача |
| **УЧАСНИКИ**     | Користувач, система |
| **ПЕРЕДУМОВИ**   | Система не зареєструвала користувача |
| **РЕЗУЛЬТАТ**    | Система створює обліковий запис користувача |
| **ВИКЛЮЧНІ СИТУАЦІЇ** | - Користувач не ввів ім'я користувача (**NullUsernameException**) <br> - Користувач не ввів пошту (**NullEmailException**) <br> - Користувач не ввів пароль (**NullPasswordException**) <br> - Користувач з таким ім'ям вже існує (**UserAlreadyExistsException**) <br> - Користувач вказав неправильний формат пошти (**WrongEmailFormatException**) <br> - Користувач ввів недостатньо сильний пароль (**WeakPasswordException**) |
| **ОСНОВНИЙ СЦЕНАРІЙ** | 1. Користувач натискає на кнопку "Зареєструватись". <br> 2. Користувач заповнює поля реєстрації (ім'я користувача, пошта, пароль). <br> 3. Користувач натискає на кнопку "Створити". <br> 4. Система перевіряє введені поля на валідність. <br> 5. Система створює обліковий запис користувача. <br> 6. Користувач автоматично входить у систему. |

@startuml

actor User
actor System

User -> System: Input username and password
System -> System: Check username and password (InvalidPasswordException, InvalidUsernameException)
System -> System: Check user status (UserBannedException)
System -> User: Successfully logged in

@enduml

| **ID**           | AuthorizeUser |
|------------------|--------------|
| **НАЗВА**        | Авторизувати користувача |
| **УЧАСНИКИ**     | Користувач, система |
| **ПЕРЕДУМОВИ**   | Система зареєструвала користувача |
| **РЕЗУЛЬТАТ**    | Система авторизувала користувача |
| **ВИКЛЮЧНІ СИТУАЦІЇ** | - Користувач ввів неправильний пароль (**InvalidPasswordException**) <br> - Користувач ввів неправильне ім'я користувача (**InvalidUsernameException**) <br> - Система заблокувала користувача (**UserBannedException**) |
| **ОСНОВНИЙ СЦЕНАРІЙ** | 1. Користувач вводить ім'я користувача і пароль. <br> 2. Система перевіряє введені дані (**InvalidPasswordException** або **InvalidUsernameException**). <br> 3. Система перевіряє статус користувача (**UserBannedException**). <br> 4. Користувач успішно входить у систему. |

@startuml

actor Admin
actor System

Admin -> System: Open user profile
Admin -> System: Edit user fields
System -> System: Check permissions (InsufficientPermissionsException)
System -> System: Validate data (InvalidDataFormatException)
System -> System: Save updated user data

@enduml

| **ID**           | EditUser |
|------------------|------------|
| **НАЗВА**        | Редагувати користувача |
| **УЧАСНИКИ**     | Користувач, адміністратор, система |
| **ПЕРЕДУМОВИ**   | Система авторизувала користувача або адміністратора |
| **РЕЗУЛЬТАТ**    | Система змінила дані користувача |
| **ВИКЛЮЧНІ СИТУАЦІЇ** | - Система не знайшла користувача (**UserNotFoundException**) <br> - Користувач має недостатньо прав для редагування (**InsufficientPermissionsException**) <br> - Користувач ввів дані у неправильному форматі (**InvalidDataFormatException**) |
| **ОСНОВНИЙ СЦЕНАРІЙ** | 1. Адміністратор або користувач відкриває профіль користувача. <br> 2. Користувач або адміністратор змінює потрібні поля. <br> 3. Система перевіряє права (**InsufficientPermissionsException**). <br> 4. Система перевіряє введені дані на правильність (**InvalidDataFormatException**). <br> 5. Система зберігає оновлені дані користувача. |

@startuml

actor Admin
actor System

Admin -> System: Select user to delete
Admin -> System: Click "Delete User"
System -> System: Check permissions (InsufficientPermissionsException)
System -> System: Delete user
System -> Admin: User deleted

@enduml

| **ID**           | DeleteUser |
|------------------|------------|
| **НАЗВА**        | Видалити користувача |
| **УЧАСНИКИ**     | Адміністратор, система |
| **ПЕРЕДУМОВИ**   | Система авторизувала адміністратора |
| **РЕЗУЛЬТАТ**    | Система видаляє користувача |
| **ВИКЛЮЧНІ СИТУАЦІЇ** | - Система не знайшла користувача (**UserNotFoundException**) <br> - Користувач має недостатньо прав для видалення (**InsufficientPermissionsException**) |
| **ОСНОВНИЙ СЦЕНАРІЙ** | 1. Адміністратор вибирає користувача для видалення. <br> 2. Адміністратор натискає кнопку "Видалити користувача". <br> 3. Система перевіряє права адміністратора (**InsufficientPermissionsException**). <br> 4. Система видаляє користувача (**UserNotFoundException**). |

@startuml

actor User
actor System

User -> System: Click "Create Project"
User -> System: Fill project form (project name)
System -> System: Validate input (NullProjectNameException, InvalidProjectNameException)
System -> System: Create project
System -> User: Assign project manager rights
System -> User: Confirmation message

@enduml

| **ID**           | CreateProject |
|------------------|------------|
| **НАЗВА**        | Створити проект |
| **УЧАСНИКИ**     | Користувач, система |
| **ПЕРЕДУМОВИ**   | Система авторизувала користувача |
| **РЕЗУЛЬТАТ**    | Система створює проєкт та надає права керівника проєкту користувачу |
| **ВИКЛЮЧНІ СИТУАЦІЇ** | - Користувач не ввів назву проєкту (**NullProjectNameException**) <br> - Користувач ввів назву проєкту у неправильному форматі (**InvalidProjectNameException**) |
| **ОСНОВНИЙ СЦЕНАРІЙ** | 1. Користувач натискає кнопку "Створити проект". <br> 2. Користувач заповнює форму (назва проекту). <br> 3. Система перевіряє дані на валідність. <br> 4. Система створює новий проект. <br> 5. Система надає права керівника проєкту користувачу. <br> 6. Користувач отримує підтвердження про створення проекту. |

@startuml

actor User
actor System

User -> System: Open project
User -> System: Click "Edit"
User -> System: Modify project details
System -> System: Check permissions (AccessDeniedException)
System -> System: Save changes

@enduml

| **ID**            | EditProject |
|------------------|-----------------|
| **НАЗВА**         | Редагувати проект |
| **УЧАСНИКИ**     | Користувач (керівник проєкту), адміністратор, система |
| **ПЕРЕДУМОВИ**   | - Система авторизувала користувача <br> - Користувач має права на редагування проекту |
| **РЕЗУЛЬТАТ**    | Система змінює дані проєкту |
| **ВИКЛЮЧНІ СИТУАЦІЇ** | - Система не знайшла проєкт (**ProjectNotFoundException**) <br> - Користувач має недостатньо прав для редагування (**AccessDeniedException**) |
| **ОСНОВНИЙ СЦЕНАРІЙ** | 1. Користувач відкриває проект. <br> 2. Користувач натискає кнопку "Редагувати". <br> 3. Користувач вносить зміни. <br> 4. Система перевіряє права на редагування. <br> 5. Система зберігає зміни. |

@startuml

actor User
actor System

User -> System: Select project to delete
User -> System: Click "Delete Project"
System -> System: Check permissions (AccessDeniedException)
System -> System: Delete project

@enduml

| **ID**            | DeleteProject |
|------------------|-----------------|
| **НАЗВА**         | Видалити проект |
| **УЧАСНИКИ**     | Користувач (керівник проєкту), адміністратор, система |
| **ПЕРЕДУМОВИ**   | - Система авторизувала користувача <br> - Користувач має права на видалення проєкту |
| **РЕЗУЛЬТАТ**    | Система видаляє проєкт |
| **ВИКЛЮЧНІ СИТУАЦІЇ** | - Система не знайшла проєкт (**ProjectNotFoundException**) <br> - Користувач має недостатньо прав для видалення (**AccessDeniedException**) |
| **ОСНОВНИЙ СЦЕНАРІЙ** | 1. Користувач вибирає проект для видалення. <br> 2. Користувач натискає кнопку "Видалити". <br> 3. Система перевіряє права на видалення. <br> 4. Система видаляє проект. |

@startuml

actor User
actor System

User -> System: Open project
User -> System: Click "Add Participant"
User -> System: Input participant details
System -> System: Check permissions (AccessDeniedException)
System -> System: Add participant to project
System -> User: Participant added

@enduml

| **ID**            | AddUserToProject |
|------------------|-----------------|
| **НАЗВА**         | Додати учасника до проекту |
| **УЧАСНИКИ**     | Користувач (керівник проєкту), адміністратор, система |
| **ПЕРЕДУМОВИ**   | - Система авторизувала користувача <br> - Користувач має права на редагування проекту |
| **РЕЗУЛЬТАТ**    | Система додає учасника до проєкту |
| **ВИКЛЮЧНІ СИТУАЦІЇ** | - Система не знайшла користувача (**UserNotFoundException**) <br> - Система не знайшла проєкт (**ProjectNotFoundException**) <br> - Користувач має недостатньо прав для додавання учасника (**AccessDeniedException**) |
| **ОСНОВНИЙ СЦЕНАРІЙ** | 1. Користувач відкриває проект. <br> 2. Користувач натискає кнопку "Додати учасника". <br> 3. Користувач вводить дані нового учасника. <br> 4. Система перевіряє права на додавання учасника. <br> 5. Система додає учасника до проекту. |

@startuml

actor Manager
actor System

Manager -> System: Open project
Manager -> System: Click "Remove User"
System -> System: Open remove user form
Manager -> System: Input username
System -> System: Validate input (RemoveUserFromProject_WrongUsername_EXC)
Manager -> System: Click "Remove"
System -> System: Remove user from project
System -> Manager: User removed

@enduml

| **ID**             | RemoveUserFromProject |
|--------------------|----------------------|
| **НАЗВА**         | Видалити користувача з проєкту |
| **УЧАСНИКИ**      | Менеджер, система |
| **ПЕРЕДУМОВИ**    | - Користувач є учасником проєкту |
| **РЕЗУЛЬТАТ**     | Видалений член проєкту |
| **ВИКЛЮЧНІ СИТУАЦІЇ** | - **RemoveUserFromProject_WrongUsername_EXC** – менеджер ввів неправильне ім'я користувача <br> - **RemoveUserFromProject_CancelButton_EXC** – менеджер натиснув кнопку "Відміна" |
| **ОСНОВНИЙ СЦЕНАРІЙ** | 1. Менеджер переходить у розділ "Проєкти". <br> 2. Менеджер обирає проєкт і натискає кнопку "Видалити користувача". <br> 3. Система відкриває форму для введення ім'я користувача. <br> 4. Менеджер вводить ім'я користувача (**можлива RemoveUserFromProject_WrongUsername_EXC**). <br> 5. Менеджер натискає кнопку "Видалити" (**можлива RemoveUserFromProject_CancelButton_EXC**). <br> 6. Система видаляє користувача з проєкту. <br> 7. Система закриває форму. <br> 8. Система показує повідомлення, що користувач успішно видалений з обраного проєкту. |

