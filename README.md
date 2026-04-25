# QLess Platform 🚀

## Multi-Tenant Event Ticketing & Digital Wallet Platform

### Overview
QLess is a complete platform for event ticketing, digital wallet payments, and Shisha lounge management. Built for South African businesses.

### Features

#### 🎟️ Event Ticketing
- Multi-tier ticket types (Early Bird, VIP, General)
- QR code tickets
- Wallet payments
- Multi-channel sales (Web, Kiosk, WhatsApp)

#### 💰 Digital Wallet
- Instant deposits via PayFast/Yoco
- Wallet-to-wallet transfers (coming soon)
- Transaction history
- PIN protection

#### 🍽️ Shisha Management
- Product inventory (flavors, coals, accessories)
- Active session tracking (per table)
- Staff assignment
- Customer rating system

#### 📱 Multi-Channel Ordering
- **Web App** - Full customer portal
- **Kiosk** - Self-service ordering
- **WhatsApp** - Order via chat + payment link
- **Staff App** - Check-in scanner + order management

#### 🏢 Multi-Tenant Architecture
- **Super Admin** - Platform owner
- **Tenant Admin** - Business owner
- **Staff** - Counter/kitchen/check-in
- **Customer** - End users with wallets

#### 📲 PWA Installable
- One-click install to home screen
- Works offline
- Push notifications
- App-like experience

### Tech Stack

| Layer | Technology |
|-------|------------|
| Frontend | React 18 + Vite |
| Backend | Laravel 11 |
| Database | Supabase (PostgreSQL) |
| Payments | PayFast, Yoco |
| SMS | Twilio |
| Hosting | DigitalOcean + Vercel |

### Database Schema (34 Tables)

<details>
<summary>Click to view all tables</summary>

**Core Tables (7)**
- tenants, users, events, ticket_types, tickets
- wallet_transactions, check_in_logs

**Shisha Module (4)**
- shisha_products, shisha_sessions, shisha_inventory
- inventory_transactions

**Order Module (3)**
- orders, order_items, order_pickup_codes

**Kiosk Module (3)**
- kiosk_devices, kiosk_orders, kiosk_ticket_orders

**WhatsApp Module (4)**
- whatsapp_sessions, whatsapp_messages
- whatsapp_ticket_orders, whatsapp_order_links

**Super Admin Module (8)**
- super_admin_settings, platform_analytics, system_logs
- audit_logs, platform_fees, announcements
- support_tickets, support_ticket_replies

**Staff & Invitations (3)**
- tenant_staff, user_invitations, tenant_activity_log

**Utility (1)**
- ticket_holds

</details>

### Stored Procedures

- `purchase_tickets_with_wallet` - Instant ticket purchase
- `purchase_tickets_at_kiosk` - Kiosk ticket sales
- `create_whatsapp_ticket_order` - WhatsApp order creation
- `complete_whatsapp_ticket_payment` - Complete payment
- `create_new_tenant` - Super Admin creates tenant
- `create_tenant_staff` - Tenant Admin creates staff

### Project Structure
