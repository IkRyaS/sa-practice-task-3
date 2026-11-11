**Словарь данных**

#### Hotel
| Поле | Тип | Ограничение | Описание |
|---|---|---|---|
| hotel_id | UUID | PK | Идентификатор отеля |
| name | VARCHAR(255) | NOT NULL | Название отеля |
| city | VARCHAR(100) | NOT NULL | Город |
| address | VARCHAR(255) | | Адрес |
| status | VARCHAR(20) | NOT NULL | Статус (активен/архив) |
| created_at | TIMESTAMP | NOT NULL | Дата создания |

#### RoomCategory
| Поле | Тип | Ограничение | Описание |
|---|---|---|---|
| category_id | UUID | PK | Идентификатор категории |
| hotel_id | UUID | FK → Hotel | Отель |
| name | VARCHAR(100) | NOT NULL | Название категории |
| capacity | INT | > 0 | Вместимость |
| description | TEXT | | Описание |

#### Quota
| Поле | Тип | Ограничение | Описание |
|---|---|---|---|
| quota_id | UUID | PK | Идентификатор квоты |
| category_id | UUID | FK → RoomCategory | Категория номера |
| date | DATE | NOT NULL | Дата |
| total | INT | ≥ 0 | Всего номеров |
| sold | INT | ≥ 0, ≤ total | Продано |
| available | INT | ≥ 0 | Доступно |
| updated_at | TIMESTAMP | NOT NULL | Дата обновления |

#### RatePlan
| Поле | Тип | Ограничение | Описание |
|---|---|---|---|
| rate_plan_id | UUID | PK | Идентификатор тарифа |
| category_id | UUID | FK → RoomCategory | Категория |
| name | VARCHAR(100) | NOT NULL | Название тарифа |
| meal_plan | VARCHAR(50) | | План питания |
| cancellation_policy | TEXT | | Правила отмены |
| valid_from | DATE | NOT NULL | Начало действия |
| valid_to | DATE | NOT NULL | Конец действия |

#### Price
| Поле | Тип | Ограничение | Описание |
|---|---|---|---|
| price_id | UUID | PK | Идентификатор цены |
| rate_plan_id | UUID | FK → RatePlan | Тариф |
| date | DATE | NOT NULL | Дата |
| amount | DECIMAL(10,2) | ≥ 0 | Сумма |
| currency | VARCHAR(3) | NOT NULL | Валюта |

#### SalesChannel
| Поле | Тип | Ограничение | Описание |
|---|---|---|---|
| channel_id | UUID | PK | Идентификатор канала |
| name | VARCHAR(100) | NOT NULL | Название канала |
| type | VARCHAR(50) | NOT NULL | Тип (OTA, сайт и т.д.) |
| is_active | BOOLEAN | NOT NULL | Активен |

#### Publication
| Поле | Тип | Ограничение | Описание |
|---|---|---|---|
| publication_id | UUID | PK | Идентификатор публикации |
| channel_id | UUID | FK → SalesChannel | Канал |
| quota_id | UUID | FK → Quota | Квота |
| published_at | TIMESTAMP | NOT NULL | Дата публикации |
| status | VARCHAR(20) | NOT NULL | Статус (успех/ошибка) |
| error_message | TEXT | | Текст ошибки |

#### StopSale
| Поле | Тип | Ограничение | Описание |
|---|---|---|---|
| stop_sale_id | UUID | PK | Идентификатор stop-sale |
| category_id | UUID | FK → RoomCategory | Категория |
| start_date | DATE | NOT NULL | Начало |
| end_date | DATE | NOT NULL | Конец |
| reason | TEXT | | Причина |
| created_by | UUID | FK → User | Автор |

#### QuotaChange
| Поле | Тип | Ограничение | Описание |
|---|---|---|---|
| change_id | UUID | PK | Идентификатор изменения |
| quota_id | UUID | FK → Quota | Квота |
| user_id | UUID | FK → User | Пользователь |
| old_value | INT | ≥ 0 | Старое значение |
| new_value | INT | ≥ 0 | Новое значение |
| changed_at | TIMESTAMP | NOT NULL | Дата изменения |
| comment | TEXT | | Комментарий |

#### User
| Поле | Тип | Ограничение | Описание |
|---|---|---|---|
| user_id | UUID | PK | Идентификатор пользователя |
| email | VARCHAR(255) | UNIQUE, NOT NULL | Email |
| full_name | VARCHAR(255) | NOT NULL | ФИО |
| role_id | UUID | FK → Role | Роль |
| is_active | BOOLEAN | NOT NULL | Активен |

#### Role
| Поле | Тип | Ограничение | Описание |
|---|---|---|---|
| role_id | UUID | PK | Идентификатор роли |
| name | VARCHAR(50) | NOT NULL | Название роли |
| description | TEXT | | Описание |

#### OverbookingIncident
| Поле | Тип | Ограничение | Описание |
|---|---|---|---|
| incident_id | UUID | PK | Идентификатор инцидента |
| category_id | UUID | FK → RoomCategory | Категория |
| date | DATE | NOT NULL | Дата |
| sold_count | INT | ≥ 0 | Продано |
| quota_count | INT | ≥ 0 | Квота |
| resolved_at | TIMESTAMP | | Дата решения |
| resolution | TEXT | | Решение |