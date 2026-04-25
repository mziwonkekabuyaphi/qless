# Database Schema Documentation

## Total Tables: 34

### Core Tables

#### tenants
```sql
- id (BIGSERIAL, PRIMARY KEY)
- business_name (TEXT, NOT NULL)
- email (TEXT, UNIQUE, NOT NULL)
- subdomain (TEXT, UNIQUE, NOT NULL)
- logo (TEXT)
- wallet_balance (DECIMAL(12,2), DEFAULT 0)
- settings (JSONB)
- status (TEXT, DEFAULT 'pending')
- created_at (TIMESTAMP, DEFAULT NOW())
```

#### users
```sql
- id (BIGSERIAL, PRIMARY KEY)
- tenant_id (BIGINT, FOREIGN KEY → tenants.id)
- name (TEXT, NOT NULL)
- email (TEXT, UNIQUE, NOT NULL)
- phone (TEXT)
- role (TEXT, DEFAULT 'customer')
- wallet_balance (DECIMAL(12,2), DEFAULT 0)
- pin (TEXT)
- created_at (TIMESTAMP, DEFAULT NOW())
```

#### events
```sql
- id (BIGSERIAL, PRIMARY KEY)
- tenant_id (BIGINT, FOREIGN KEY → tenants.id)
- name (TEXT, NOT NULL)
- slug (TEXT, UNIQUE, NOT NULL)
- description (TEXT)
- start_date (TIMESTAMP, NOT NULL)
- end_date (TIMESTAMP, NOT NULL)
- venue (TEXT)
- city (TEXT)
- total_capacity (INTEGER, NOT NULL)
- tickets_sold (INTEGER, DEFAULT 0)
- status (TEXT, DEFAULT 'draft')
- created_at (TIMESTAMP, DEFAULT NOW())
```

#### ticket_types
```sql
- id (BIGSERIAL, PRIMARY KEY)
- event_id (BIGINT, FOREIGN KEY → events.id)
- name (TEXT, NOT NULL)
- price (DECIMAL(10,2), NOT NULL)
- quantity_available (INTEGER, NOT NULL)
- quantity_sold (INTEGER, DEFAULT 0)
- max_per_order (INTEGER, DEFAULT 10)
- benefits (JSONB)
- created_at (TIMESTAMP, DEFAULT NOW())
```

#### tickets
```sql
- id (BIGSERIAL, PRIMARY KEY)
- ticket_number (TEXT, UNIQUE, NOT NULL)
- qr_code (TEXT, UNIQUE, NOT NULL)
- user_id (BIGINT, FOREIGN KEY → users.id)
- event_id (BIGINT, FOREIGN KEY → events.id)
- ticket_type_id (BIGINT, FOREIGN KEY → ticket_types.id)
- holder_name (TEXT, NOT NULL)
- amount_paid (DECIMAL(10,2), NOT NULL)
- status (TEXT, DEFAULT 'active')
- channel (TEXT) -- 'wallet', 'kiosk', 'whatsapp'
- checked_in_at (TIMESTAMP)
- created_at (TIMESTAMP, DEFAULT NOW())
```

#### wallet_transactions
```sql
- id (BIGSERIAL, PRIMARY KEY)
- user_id (BIGINT, FOREIGN KEY → users.id)
- type (TEXT, NOT NULL) -- 'deposit', 'purchase', 'refund'
- amount (DECIMAL(12,2), NOT NULL)
- balance_before (DECIMAL(12,2), NOT NULL)
- balance_after (DECIMAL(12,2), NOT NULL)
- reference (TEXT)
- created_at (TIMESTAMP, DEFAULT NOW())
```

### Shisha Module Tables

#### shisha_products
```sql
- id (BIGSERIAL, PRIMARY KEY)
- tenant_id (BIGINT, FOREIGN KEY → tenants.id)
- name (TEXT, NOT NULL)
- category (TEXT, NOT NULL) -- 'fruit', 'mint', 'premium'
- price (DECIMAL(10,2), NOT NULL)
- is_available (BOOLEAN, DEFAULT TRUE)
- preparation_time (INTEGER, DEFAULT 10)
- created_at (TIMESTAMP, DEFAULT NOW())
```

#### shisha_sessions
```sql
- id (BIGSERIAL, PRIMARY KEY)
- order_id (BIGINT, FOREIGN KEY → orders.id)
- table_number (TEXT)
- customer_name (TEXT)
- status (TEXT, DEFAULT 'active')
- started_at (TIMESTAMP, DEFAULT NOW())
- assigned_staff_id (BIGINT, FOREIGN KEY → users.id)
- rating (INTEGER)
- created_at (TIMESTAMP, DEFAULT NOW())
```

### Stored Procedures

```sql
-- Purchase tickets with wallet balance
purchase_tickets_with_wallet(
    p_user_id BIGINT,
    p_ticket_type_id BIGINT,
    p_quantity INTEGER,
    p_holder_name TEXT,
    p_holder_email TEXT,
    p_holder_phone TEXT
)

-- Purchase tickets at kiosk
purchase_tickets_at_kiosk(
    p_tenant_id BIGINT,
    p_ticket_type_id BIGINT,
    p_quantity INTEGER,
    p_customer_phone TEXT,
    p_customer_name TEXT,
    p_kiosk_device_id BIGINT,
    p_customer_email TEXT DEFAULT NULL,
    p_payment_method TEXT DEFAULT 'wallet'
)

-- Create WhatsApp ticket order
create_whatsapp_ticket_order(
    p_tenant_id BIGINT,
    p_ticket_type_id BIGINT,
    p_quantity INTEGER,
    p_customer_phone TEXT,
    p_customer_name TEXT,
    p_customer_email TEXT DEFAULT NULL,
    p_whatsapp_session_id BIGINT DEFAULT NULL
)

-- Complete WhatsApp payment
complete_whatsapp_ticket_payment(
    p_hold_code TEXT,
    p_payment_method TEXT DEFAULT 'wallet'
)

-- Create new tenant (Super Admin)
create_new_tenant(
    p_business_name TEXT,
    p_subdomain TEXT,
    p_admin_email TEXT,
    p_admin_name TEXT,
    p_admin_phone TEXT,
    p_created_by BIGINT
)

-- Create staff member (Tenant Admin)
create_tenant_staff(
    p_tenant_id BIGINT,
    p_staff_name TEXT,
    p_staff_email TEXT,
    p_staff_phone TEXT,
    p_staff_role TEXT,
    p_created_by BIGINT
)
```

### Views

#### kitchen_display_orders
Shows active orders for kitchen TV display with items grouped by order status.
