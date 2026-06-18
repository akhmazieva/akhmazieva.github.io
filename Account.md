erDiagram
    USERS {
        int user_id PK
        string username
        string email
        string password_hash
        datetime created_at
        boolean is_active
    }

    GROUPS {
        int group_id PK
        string name
        text description
    }

    AUTHENTICATION_AND_AUTHORIZATION {
        int auth_id PK
        string session_token
        datetime last_login
        datetime expires_at
        int user_id FK
    }

    ADMINS {
        int admin_id PK
        int user_id FK "ссылка на Users"
        string role
        datetime assigned_at
    }

    ACCOUNTS {
        int account_id PK
        string name
        decimal balance
        string account_type
        boolean is_active
        int user_id FK
        int currency_id FK
    }

    BILLS {
        int bill_id PK
        string title
        decimal amount
        date due_date
        boolean is_paid
        int user_id FK
        int category_id FK
    }

    BUDGETS {
        int budget_id PK
        date period_start
        date period_end
        decimal limit
        text description
        int user_id FK
        int category_id FK
    }

    CATEGORIES {
        int category_id PK
        string name
        string category_type "income/expense"
        int parent_id FK "nullable"
    }

    CURRENCIES {
        int currency_id PK
        string code "RUB, USD, EUR"
        string name "Russian Ruble, US Dollar"
        string symbol "₽, $, €"
    }

    TRANSACTIONS {
        int transaction_id PK
        string transaction_type "income, expense, transfer"
        decimal amount
        date date
        text description
        int account_id FK
        int category_id FK "nullable"
        int target_account_id FK "nullable"
    }

    USERS ||--o{ GROUPS : "belongs_to"
    USERS ||--o{ AUTHENTICATION_AND_AUTHORIZATION : "has"
    USERS ||--o{ ADMINS : "may_be"
    USERS ||--o{ ACCOUNTS : "owns"
    USERS ||--o{ BILLS : "has"
    USERS ||--o{ BUDGETS : "creates"
    USERS ||--o{ TRANSACTIONS : "makes"

    GROUPS ||--o{ USERS : "contains"

    CURRENCIES ||--o{ ACCOUNTS : "used_in"

    CATEGORIES ||--o{ CATEGORIES : "parent_of"
    CATEGORIES ||--o{ BILLS : "applies_to"
    CATEGORIES ||--o{ BUDGETS : "applies_to"
    CATEGORIES ||--o{ TRANSACTIONS : "classifies"

    ACCOUNTS ||--o{ TRANSACTIONS : "source_of"
    ACCOUNTS ||--o{ TRANSACTIONS : "target_of" "for transfers"
