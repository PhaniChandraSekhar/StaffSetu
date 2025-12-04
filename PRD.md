This is a **technical PRD (Product Requirements Document)** optimized for AI Coding Assistants (like **Cursor**, **Windsurf**, **Replit Agent**, or **Bolt.new**).

These tools work best when you provide **strict constraints**, **database schemas**, and **step-by-step user flows**.

You can copy-paste the text below directly into your coding assistant's chat window.

***

# Project Name: StaffSetu (MVP)
**Type:** Web Application (Mobile-First)
**Context:** A digital identity and verification platform for blue-collar workers in India.

## 1. High-Level Objective
Build a mobile-first web app that allows:
1.  **Admins (Field Agents)** to onboard workers (upload photo, details) and generate a public profile URL.
2.  **Public/Business Owners** to scan a QR code (which links to the profile URL) to view a worker's details and "Vouch" for them.
3.  **Workers** to have a digital "Trust Profile" that proves their identity and skills.

## 2. Tech Stack Recommendations
* **Framework:** Next.js 14 (App Router) - for SEO and speed.
* **Language:** TypeScript.
* **Styling:** Tailwind CSS + Shadcn UI (for clean, accessible components).
* **Backend/Database:** Supabase (PostgreSQL) - handles Auth, Database, and Image Storage.
* **Icons:** Lucide React.
* **QR Code Library:** `react-qr-code` or `qrcode.react`.

## 3. Core User Roles
1.  **Super Admin / Field Agent:**
    * Has password-protected access to the dashboard.
    * Can create/edit worker profiles.
    * Can print/download QR codes.
2.  **Public User (Employer/Business Owner):**
    * No login required to *view* profiles.
    * Simple OTP login (optional for MVP, or just simple form) to *Vouch* for a worker.

## 4. Database Schema (Supabase)

**Table: `workers`**
* `id` (uuid, primary key)
* `full_name` (text)
* `phone_number` (text) -> *Mark as Private (do not display on public frontend)*
* `skill_category` (text) -> e.g., "Tandoor Cook", "Waiter", "Housekeeping"
* `profile_photo_url` (text) -> URL from Supabase Storage
* `status` (text) -> Options: 'Pending', 'Verified', 'Blacklisted'
* `staff_id_code` (text, unique) -> e.g., "HYD-001"
* `created_at` (timestamp)
* `is_insured` (boolean) -> Default: false

**Table: `vouches`**
* `id` (uuid, primary key)
* `worker_id` (foreign key linked to `workers.id`)
* `voucher_name` (text) -> Name of the business owner
* `voucher_business` (text) -> Name of the restaurant/shop
* `voucher_phone` (text) -> For verification
* `created_at` (timestamp)

## 5. Feature Requirements & User Flows

### Feature A: Field Agent Dashboard (Protected Route `/admin`)
1.  **Login:** Simple Email/Password login (Supabase Auth).
2.  **Add Worker Form:**
    * Inputs: Name, Phone, Skill (Dropdown), Status.
    * Image Upload: Cropper tool to upload a square headshot to Supabase Storage.
    * Submit Button: "Generate Profile".
3.  **Worker List View:**
    * Table showing all workers.
    * Action: "Print Card" view (generates a printable HTML layout of the physical ID card with the dynamic QR code).

### Feature B: Public Profile Page (Dynamic Route `/p/[worker_id]`)
* **Design:** Mobile-optimized, clean, "Official Document" feel.
* **Header:** StaffSetu Logo + "Official Work Passport".
* **Hero Section:** Large Photo + Name + Skill + "Verified" Badge (Green Checkmark).
* **Details:**
    * Staff ID: [Code]
    * Location: Hyderabad
    * Insurance Status: "Active" (Green shield icon if `is_insured` = true).
* **Call to Action:**
    * **Primary Button:** "Vouch for this Worker" (Trust Button).
    * **Secondary Button:** "Request to Hire" (Opens a WhatsApp link to the Admin's number with a pre-filled message: "I am interested in hiring [Worker Name]"). *Note: Do not show Worker's phone number directly.*

### Feature C: The Vouch System
1.  User clicks "Vouch for this Worker" on the profile page.
2.  Modal opens asking: "Are you a Business Owner?"
3.  User enters Name, Shop Name, and Phone Number.
4.  System saves entry to `vouches` table.
5.  Page updates to show "Total Vouches: X".

## 6. UI/Design Guidelines (Tailwind)
* **Primary Color:** "Trust Blue" (`bg-blue-700` / `#0F4C81`).
* **Secondary Color:** "Verified Green" (`bg-green-600`).
* **Typography:** Sans-serif (Inter or Roboto).
* **Language Support:** Interface in English, but allow space in UI for bilingual labels (e.g., Name / పేరు).

## 7. Instructions for the AI Assistant
1.  **Step 1:** Set up the Next.js project structure with Shadcn UI.
2.  **Step 2:** configure the Supabase client and creating the migration SQL script for the tables defined above.
3.  **Step 3:** Build the `/admin` dashboard first to enable data entry.
4.  **Step 4:** Build the dynamic `/p/[id]` public profile page.
5.  **Step 5:** Implement the Vouching logic.

***

### **How to use this:**
1.  Open **Cursor** (or your preferred AI tool).
2.  Start a **"New Project"**.
3.  Paste the text above into the chat.
4.  **Prompt:** *"I want to build this MVP. Please start by helping me set up the project structure and the Supabase database migration file based on the schema provided."*