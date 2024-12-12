## 1. Составьте список запросов, которые отвечают за чтение данных:

- Получение проекта по ID
- Получение проектов пользователя
- Получение задачи по ID
- Получение задач в проекте
- Получение задач пользователя
- Получение данных пользователя
- Получение списка статусов в проекте

---

### 2. Для каждой операции обозначьте список ограниченных контекстов, которые в ней задействованы:

- Получение проекта по ID: Контекст проекта
- Получение проектов пользователя: Контекст проекта
- Получение задачи по ID: Контекст проекта
- Получение задач в проекте: Контекст проекта
- Получение задач пользователя: Контекст пользователя
- Получение данных пользователя: Контекст пользователя
- Получение списка статусов в проекте: Контекст проекта

---

### 3. Для каждой операции обозначьте список агрегатов, которые в ней задействованы:

- Получение проекта по ID: `ProjectAggregate`
- Получение проектов пользователя: `ProjectAggregate`
- Получение задачи по ID: `TaskAggregate`
- Получение задач в проекте: `TaskAggregate`, возможно с учетом `ProjectAggregate`
- Получение задач пользователя: `UserAggregate`
- Получение данных пользователя: `UserAggregate`
- Получение списка статусов в проекте: `StatusAggregate`

---

### 4. Опишите проекции, которые могут помочь в обслуживании запросов. Предложенные проекции:

#### Проекции и их структуры:

- **Получение проекта по ID**: `ProjectViewDomain`
- **Получение проектов пользователя**: `ProjectViewDomain`
- **Получение задачи по ID**: `TaskViewDomain`
- **Получение задач в проекте**: `ProjectViewDomain` + `TaskViewDomain`
- **Получение задач пользователя**: `TaskViewDomain`
- **Получение данных пользователя**: `UserViewDomain`
- **Получение списка статусов в проекте**: `StatusViewDomain`

**Структуры данных:**

```kotlin
// Проекция для представления проекта
data class ProjectViewDomain(
    val id: UUID,
    val name: String
)
// Проекция для представления задачи
data class TaskViewDomain(
    val id: UUID,
    val projectId: UUID,
    val taskName: String,
    val taskDescription: String,
    val statusName: String,
    val assigneeUsersId: List<UUID>
)
// Проекция для представления статуса
data class StatusViewDomain(
    val id: UUID,
    val projectId: UUID,
    val statusName: String,
    val statusColor: String,
    val orderNumber: Int
)
// Проекция для представления пользователя
data class UserViewDomain(
    val id: UUID,
    val nickname: String,
    val userName: String,
    val password: String,
    val projectsIds: List<UUID>
)
5. Дополнительное использование проекций в системе:
Для чего могут понадобиться проекции:

Ускорение обработки сложных запросов для больших проектов.
Получение агрегированных данных, например, количества задач в проекте, их статусов или пользователей.
Быстрая фильтрация данных для пользовательских интерфейсов.
Примеры дополнительных проекций:

@Document("project-view")
data class ProjectView(
    @Id
    val id: UUID,
    var name: String,
    var taskIds: MutableList<UUID> = mutableListOf(),
    var userIds: MutableList<UUID> = mutableListOf()
)
@Document("task-view")
data class TaskView(
    @Id
    val id: UUID,
    val projectId: UUID,
    var taskName: String,
    var taskDescription: String,
    var statusName: String,
    var statusColor: String,
    var assigneeIds: MutableList<UUID> = mutableListOf()
)
@Document("user-view")
data class UserView(
    @Id
    val id: UUID,
    var nickname: String,
    val userName: String,
    val password: String,
    val projectsIds: MutableList<UUID> = mutableListOf(),
    val tasksAssignedIds: MutableList<UUID> = mutableListOf()
)
