# Secure SaaS CRM Patterns (Supabase + Retool)

This repository documents practical, reusable patterns I used while building
a production CRM SaaS platform for UK small businesses.

The focus is on **security, multi-user access, and real-world SaaS architecture**,
rather than theoretical examples.

---

## Overview

The platform was designed as a secure, multi-user CRM system allowing UK SMEs
to manage customer data, orders, and operational workflows in one place.

Key goals:
- Secure data access
- Simplicity for non-technical users
- Scalability for future growth

---

## Architecture Summary

- Frontend: Retool (internal tools & dashboards)
- Backend: Supabase (PostgreSQL + Auth + RLS)
- Security: Row Level Security (RLS)
- Hosting: Cloud-based SaaS deployment

All access control is enforced at the **database level**, not just in the UI.

---

## Database Design

The database uses a relational schema covering:
- Customers
- Orders
- Scheduling
- Users and roles

The schema is structured to reflect real business workflows rather than generic CRM models.

---

## Data Isolation Model

This SaaS is designed with **database-per-client isolation** in Supabase.
Each client operates in a separate Supabase project/database, which reduces
cross-tenant risk and simplifies access control.

Within each client database, access is enforced using RLS policies tied to
authenticated users.

---

## Row Level Security (RLS) Patterns

RLS is used to:
- Restrict users to authorised records only
- Prevent unauthorised access to sensitive business data
- Enforce access rules at the database level (not only in the UI)

Example pattern (simplified, user-scoped access):

```sql
CREATE POLICY "Users can read their own orders"
ON public.orders
FOR SELECT
USING (user_id = auth.uid());
