# Ansible Role: Minecraft Server with IndustrialCraft

📦 Ansible-роль для автоматизированной установки и настройки Minecraft сервера с модом **IndustrialCraft** на Ubuntu Server.

Поддерживает:
- Установку Java
- Скачивание и установку Forge + IndustrialCraft
- Создание systemd unit
- Оптимизацию JVM под малую оперативную память (до 2 ГБ)
- Автоматическую очистку логов через systemd timer

---

## 🚀 Требования

- Ubuntu 20.04 / 22.04 (проверено)
- Ansible >= 2.10
- Минимум 2 ГБ ОЗУ (настройки оптимизированы для low-end VPS)

---

## 📁 Структура
```bash
.
├── README.md
├── inventory.ini
├── playbook.yml
└── roles
    └── minecraft-ic2
        ├── defaults
        │   └── main.yml
        ├── files
        ├── handlers
        │   └── main.yml
        ├── meta
        │   └── main.yml
        ├── tasks
        │   ├── log_cleanup.yml
        │   └── main.yml
        ├── templates
        │   ├── clean_logs.service.j2
        │   ├── clean_logs.sh.j2
        │   ├── clean_logs.timer.j2
        │   ├── eula.txt.j2
        │   └── minecraft.service.j2
        └── vars
```
---

## ⚙️ Переменные

Можно переопределять в инвентаре или `host_vars`.

| Переменная              | Значение по умолчанию           | Описание |
|------------------------|----------------------------------|----------|
| `minecraft_version`    | `1.12.2`                         | Версия Minecraft |
| `forge_version`        | `14.23.5.2859`                   | Версия Forge |
| `ic2_url`              | `https://...`                   | URL на `.jar` мод IndustrialCraft |
| `minecraft_user`       | `minecraft`                     | Пользователь сервиса |
| `minecraft_group`      | `minecraft`                     | Группа сервиса |
| `minecraft_home`       | `/opt/minecraft`                | Путь установки |
| `memory_min`           | `512M`                          | Минимальный объём памяти JVM |
| `memory_max`           | `1536M`                         | Максимальный объём памяти JVM |
| `jvm_flags`            | `-XX:+UseG1GC ...`              | Оптимизированные флаги для low-RAM |
| `log_retention_days`   | `7`                             | Кол-во дней для хранения логов |
| `log_cleanup_time`     | `"03:00"`                       | Время ежедневной очистки логов |

---

## 🔧 Использование

### 1. Добавить в `requirements.yml`

```yaml
- src: ./minecraft_server
  name: minecraft_server
```

2. Пример site.yml
```yaml
- name: Install Minecraft server
  hosts: minecraft
  become: true
  roles:
    - minecraft_server
```
3. Пример inventory.ini
```bash
[minecraft]
your.server.ip
```


⸻

🔥 Оптимизация

JVM-флаги и Xmx/Xms настроены для VPS с 2 ГБ RAM. Используются рекомендации Aikar’s JVM flags.

⸻

🧹 Очистка логов

Каждый день в 03:00 запускается systemd-timer, который удаляет логи старше log_retention_days. Таймер и сервис автоматически создаются и активируются.

⸻

🛠 TODO / Возможности
	•	Резервное копирование мира в tar.gz
	•	Загрузка бэкапов в S3 или GDrive
	•	Установка других модов по списку

⸻

🧪 Тестирование

Локально можно тестировать с помощью molecule и docker, или развернуть на временном VPS:

ansible-playbook -i inventory.ini site.yml



⸻

🧾 Лицензия

MIT License — используй и модифицируй свободно 🎮

⸻


