# Конфигурации

Данная конфигурация написана на **HashiCorp Configuration Language (HCL)** — это собственный язык конфигурации, используемый в инструментах HashiCorp, таких как **Terraform** и **Nomad**.

### **На что похож HCL?**
HCL сочетает в себе черты нескольких популярных форматов:

1. **JSON-подобная структура** (вложенные блоки, ключи-значения)
    - Например:
      ```hcl
      resource "type" "name" {
        key = "value"
      }
      ```  
    - Напоминает JSON, но с более человекочитаемым синтаксисом.

2. **YAML-подобная читаемость** (отступы, отсутствие кавычек для ключей)
    - Например, в YAML:
      ```yaml
      quotas:
        - resource_name: "compute_cores"
          resource_quotas:
            - region: "ru-1"
              value: 4
      ```  
    - В HCL это выглядит похоже, но с фигурными скобками.

3. **INI-подобные секции** (блоки `resource`, `variable`, `output`)
    - Например:
      ```ini
      [resource.selectel_vpc_project_v2.webservice]
      name = "webservice"
      ```  
    - В HCL блоки определяются через `resource "type" "name" { ... }`.

4. **Заимствования из языков программирования**
    - Интерполяция переменных (`${...}`) — как в Bash/PHP.
    - Зависимости (`depends_on`) — как в системах сборки (Makefile, Gradle).

### **Чем HCL отличается от других форматов?**
| Характеристика  | HCL (Terraform) | JSON | YAML | INI |
|----------------|----------------|------|------|-----|
| **Читаемость** | Высокая | Низкая (много кавычек, запятых) | Высокая | Средняя |
| **Поддержка сложных структур** | Да (вложенные блоки) | Да (но громоздко) | Да | Нет |
| **Комментарии** | `#` и `//` | Нет | Да | `;` или `#` |
| **Интерполяция переменных** | Да (`${var.name}`) | Нет | Нет (только в шаблонах) | Нет |
| **Типизация** | Динамическая + валидация | Строгая (всё — строка/число/массив) | Динамическая | Только строки |

### **Вывод**
Конфигурация Terraform (HCL) **наиболее похожа на гибрид JSON и YAML**, но с дополнительными возможностями:
- **Более выразительный синтаксис**, чем в JSON.
- **Поддержка зависимостей** (`depends_on`), чего нет в YAML.
- **Интерполяция переменных** (как в языках программирования).

Это делает HCL удобным для описания инфраструктуры, сохраняя баланс между машиночитаемостью и удобством для разработчиков.

### **Пример**

Конфигурации имеют вид
```hcl
resource "selectel_vpc_project_v2" "webservice" {
name = "webservice"

quotas {
resource_name = "compute_cores"
resource_quotas {
region = "ru-1"
zone = "ru-1a"
value = 4
}
resource_quotas {
region = "ru-2"
zone = "ru-2a"
value = 6
}
}

output "webservice_floating_ip_ru2_1_id" {
  value = "${selectel_vpc_floatingip_v2.webservice_floating_ip_ru2_1.id}"
}

output "webservice_floating_ip_ru2_1_address" {
  value = "${selectel_vpc_floatingip_v2.webservice_floating_ip_ru2_1.floating_ip_address}"
}
```

В конфигурациях имеются переменные ``${selectel_vpc_floatingip_v2.webservice_floating_ip_ru2_1.floating_ip_address}``
которые приходят из модуля ресурсов. Как ресурс опеределишь такая переменная и придёт