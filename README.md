# Bachelor Room Expense Manager - Project Draft

## Overview

An all-in-one expense manager app for bachelor/shared rooms. It allows members to record expenses, split costs monthly, settle dues via UPI, upload bill receipts, and export records to Excel. Admins control splitting, freeze months, and manage member availability.

## Tech Stack

- Flutter (UI Framework)
- Provider (State Management)
- Firebase Auth, Firestore, Storage
- UPI Integration: `upi_india` / `flutter_upi`
- Image Compression: `flutter_image_compress`
- Excel/CSV Export: `excel` or `csv` package

## Core Concepts

**Room**: A group with members  
**Admin**: Manager of the room  
**Member**: Authenticated user  
**Expense**: Amount, reason, bill, date  
**Settlement**: Dues summary for the month  
**Availability**: Member status (available/outstation)

## Authentication & User Roles

- Login with Firebase Auth
- Unique UID per user
- User can join multiple rooms
- Store user's rooms in profile

## Room & Member Management

- Create/Join rooms
- Admin assigned on room creation
- Members listed with availability
- Default availability: true (available)
- Members can update availability within 2-day grace period (1st-2nd)
- Admin can override anytime

## Expense Recording

- Members add: amount, reason, bill image, shared-with
- Bill image compressed to <2MB and uploaded
- Expense stored under Firestore with date
- Admin can edit/override split details

## Monthly Splitting & Freezing

- On month-end, app prompts admin to split
- Splitting considers only available users
- Splits saved in settlements collection
- Admin freezes month: no more edits
- Trigger Excel export and sharing options

## UPI Payment & Tracking

- For each due, show 'Settle Up' with UPI details
- Launch GPay/PhonePe/Paytm
- Save transaction status, txn ID
- Track who paid what, to whom

## Excel Export & Sharing

- Export frozen month data to XLS/CSV
- Auto-send to admin email (if available)
- Allow share via WhatsApp using `flutter_share` / `share_plus`

## Auto Cleanup & Notifications

- Every 6 months, delete old bill images (non-frozen months only)
- Notify admin before cleanup
- Notify users:
  - Availability reminder on 1st & 2nd
  - Payment due reminder by admin
  - Freeze/export reminders

## Firestore Design (Simplified)

users/{uid}:
name, email, joinedRooms
rooms/{roomId}:
name, adminUid, members[], expenses[], settlements[month]