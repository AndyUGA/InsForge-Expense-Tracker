# InsForge Expense Tracker: Full Tutorial Prompts

---

## 1. Build Initial App

```javascript
Create a new React app with TypeScript and Vite.
Add Tailwind CSS 3.4 for styling.
Install the InsForge SDK and set up the client configuration.

In my InsForge database, create an expenses table with the following columns:
- id (primary key)
- user_id (linked to authenticated users)
- amount (numeric)
- category (string)
- description (string)
- created_at (timestamp)

Enable authentication with email and password.

Configure row-level security so that users can only create, view, update, and delete their own expenses.

Add sample categories if needed (e.g., Food, Transportation, Entertainment).

Create a React component that:
- Allows a logged-in user to create a new expense
- Fetches and displays all expenses for the logged-in user
- Allows updating an existing expense
- Allows deleting an expense

Ensure the UI is clean and minimal, focusing primarily on demonstrating full CRUD functionality connected to the InsForge backend.
```

---

## 2. Enable Realtime Feature


```javascript
Enable real-time functionality for the expenses table in my InsForge project.

Configure real-time subscriptions so that when an expense is created, updated, or deleted, all connected clients automatically receive the changes.

Ensure that:
- Real-time updates respect row-level security (users should only receive updates for their own expenses).
- Subscriptions are scoped to the authenticated user.
- Insert, update, and delete events are handled.

Update the React application to:
- Subscribe to real-time changes after a user logs in.
- Automatically update the UI when an expense is added, modified, or removed.
- Properly clean up the subscription when the component unmounts.

Keep the implementation clean and focused on demonstrating real-time synchronization of user-specific expenses.
```

---

## 3. Create Cloud Function to Summary Monthly Expense


```javascript
Create a serverless (edge) function named getMonthlyExpenseSummary.

This function should:
- Require authentication.
- Extract the authenticated user's ID from the request context (do NOT accept user_id from the client).
- Query the expenses table.
- Filter expenses to include only those belonging to the authenticated user within the current calendar month.

The function should calculate and return the following fields:
- total: the sum of all expense amounts for the current month.
- transactionCount: the total number of expenses for the current month.
- largestExpense: the highest single expense amount for the current month.

Return a structured JSON response in the following format:
{
  "total": 1240.50,
  "transactionCount": 18,
  "largestExpense": 320.00
}

Ensure the function:
- Respects row-level security.
- Handles edge cases (no expenses found should return zeros).
- Is optimized to perform aggregation efficiently at the database level.

Then update the React frontend to:
- Call this function after login.
- Display the monthly summary above the expense list.
- Handle loading and error states cleanly.
```

---

## 4. Add Ability to Upload Images to Expenses


```javascript
Enable file storage in my InsForge project to support uploading images for expense receipts.

Storage Configuration:
- Create a storage bucket named expense-receipts.
- Restrict access: Only authenticated users can upload/read their own files.
- Limit uploads to image file types only (jpg, jpeg, png, webp).
- Enforce a 5MB file size limit.

Database Updates:
- Update the expenses table to include: receipt_url (string, nullable).
- Ensure this field respects row-level security.

Frontend Updates (React):
- Add a file input when creating or editing an expense.
- Upload the selected image to the expense-receipts bucket.
- Store the returned file URL/path in the receipt_url column.
- Display the uploaded receipt image in the expense list (thumbnail preview).
- Allow replacing or deleting the receipt image when updating an expense.

Ensure:
- Uploads are securely tied to the authenticated user.
- Storage paths are scoped by user ID (e.g., /user-id/filename.jpg).
- Proper loading and error states are handled.
```

---

## 5. Create Cloud Function to Generate Insights


```javascript
Create a serverless (edge) function named generateExpenseInsights that analyzes a user's expenses and generates AI-powered spending insights.

Authentication and Security:
- Require authentication and extract user ID from context.
- Ensure the function only accesses expenses belonging to the authenticated user.

Data Analysis Requirements:
- Query expenses for the current and previous calendar months.
- Calculate: Total spending for both months, percentage change, top spending category, and transaction count.

AI Insight Generation:
- Use AI to generate a natural language insight (e.g., "Your spending increased by 18% this month. Food is your top category...").
- The insight should be clear, concise, and reference real calculated values.

Response Format:
{
  "totalCurrentMonth": 1240.50,
  "totalPreviousMonth": 1050.00,
  "percentageChange": 18,
  "topCategory": "Food",
  "topCategoryPercentage": 42,
  "transactionCount": 18,
  "insight": "..."
}

Frontend Integration:
- Call this function after login and display the insight prominently.
- Handle loading and error states cleanly.
```

---
