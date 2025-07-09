# pass.in

pass.in is an application for **managing attendees at in-person events**.

The tool allows the organizer to register an event and open a public registration page.

Registered attendees can issue a credential for check-in on the day of the event.

The system will scan the attendee's credential to allow entry into the event.

## Requirements

### Functional Requirements

- [x] The organizer must be able to register a new event;
- [x] The organizer must be able to view data for an event;
- [x] The organizer must be able to view the list of attendees;
- [x] The attendee must be able to register for an event;
- [x] The attendee must be able to view their registration badge;
- [x] The attendee must be able to check into the event;

### Business Rules

- [x] An attendee can only register for an event once;
- [x] An attendee can only register for events with available spots;
- [x] An attendee can only check into an event once;

### Non-Functional Requirements

- [x] Event check-in will be done via a QRCode;

## API Documentation (Swagger)

For API documentation, access the link: https://nlw-unite-nodejs.onrender.com/docs

## Database

In this application, we will use a relational database (SQL). For the development environment, we will use SQLite for its ease of setup.

### ERD Diagram

<img src=".github/erd.svg" width="600" alt="Database ERD Diagram" />

### Database Structure (SQL)

```sql
-- CreateTable
CREATE TABLE "events" (
    "id" TEXT NOT NULL PRIMARY KEY,
    "title" TEXT NOT NULL,
    "details" TEXT,
    "slug" TEXT NOT NULL,
    "maximum_attendees" INTEGER
);

-- CreateTable
CREATE TABLE "attendees" (
    "id" INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
    "name" TEXT NOT NULL,
    "email" TEXT NOT NULL,
    "event_id" TEXT NOT NULL,
    "created_at" DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT "attendees_event_id_fkey" FOREIGN KEY ("event_id") REFERENCES "events" ("id") ON DELETE RESTRICT ON UPDATE CASCADE
);

-- CreateTable
CREATE TABLE "check_ins" (
    "id" INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
    "created_at" DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    "attendeeId" INTEGER NOT NULL,
    CONSTRAINT "check_ins_attendeeId_fkey" FOREIGN KEY ("attendeeId") REFERENCES "attendees" ("id") ON DELETE RESTRICT ON UPDATE CASCADE
);

-- CreateIndex
CREATE UNIQUE INDEX "events_slug_key" ON "events"("slug");

-- CreateIndex
CREATE UNIQUE INDEX "attendees_event_id_email_key" ON "attendees"("event_id", "email");

-- CreateIndex
CREATE UNIQUE INDEX "check_ins_attendeeId_key" ON "check_ins"("attendeeId");
```
