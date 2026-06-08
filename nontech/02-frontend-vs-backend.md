# Frontend vs Backend

## What

A web application has two main parts: the frontend (what users see and interact with) and the backend (the logic and data behind the scenes). Understanding the difference helps you communicate with technical teams and make better decisions.

## The Restaurant Analogy

Think of a restaurant:

### Frontend — The Dining Room

The dining room is what customers experience directly.

- The menu and how it's laid out
- The tables, chairs, and decor
- How the waiter takes your order
- How the food is presented on the plate

In software terms: the website or app interface. Buttons, forms, animations, colors, layout. The frontend runs on your device (phone, laptop).

### Backend — The Kitchen

The kitchen is where the actual work happens, out of sight.

- The chef preparing the food
- The recipes (business logic)
- The pantry where ingredients are stored (database)
- The system for tracking orders

In software terms: the servers, databases, and code that process requests, enforce rules, and store data. The backend runs on servers in data centers.

### The Pantry — The Database

Where all the ingredients (data) are stored. The backend reads from and writes to the database, just like the chef fetches ingredients from the pantry.

## How They Work Together

```
User taps "Place Order" (Frontend)
    → Request sent to server (Backend)
        → Backend validates the order
        → Backend checks inventory (Database)
        → Backend processes payment
        → Backend stores the order (Database)
    → Response sent back to user (Frontend)
User sees "Order confirmed!" (Frontend)
```

## What Frontend Developers Do

- Build user interfaces (pages, forms, buttons)
- Ensure the app looks good on different devices (phone, tablet, desktop)
- Handle user interactions (clicking, scrolling, typing)
- Optimize for speed and smooth experience
- Technologies: HTML, CSS, JavaScript, React, Vue, Angular

## What Backend Developers Do

- Build APIs (the rules for how frontend and backend communicate)
- Implement business logic (pricing, permissions, workflows)
- Manage databases (storing and retrieving data)
- Handle security (authentication, authorization, encryption)
- Integrate with external services (payment providers, email, AI)
- Ensure the system is reliable and scales under load
- Technologies: Python, Java, Go, Node.js, PostgreSQL, Redis

## Key Takeaway

The frontend is about user experience. The backend is about data, logic, and reliability. Both are essential. A beautiful frontend with a broken backend serves no one. A solid backend with a confusing frontend gets no users.

When prioritizing, ask: "Is the problem about what users see and do (frontend) or about data and processing (backend)?"
