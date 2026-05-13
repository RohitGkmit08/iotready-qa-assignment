# IoTReady QA Engineer — Take-Home Assessment

## Part 1 — Bug Report

### Internal Bug Report

**Title:** App intermittently stops working on customer device

**Summary:**  
Customer reports that the app "doesn't work sometimes." Issue has not been reproduced internally yet.

**Current Status:**  
Unable to reproduce on test devices so far.

**Impact:**  
Unknown. Potential intermittent usability or stability issue affecting customer workflow.

**Environment:**  
Not yet confirmed by customer.

**Frequency:**  
Intermittent.

**Steps to Reproduce:**  
Not available yet.

**Expected Result:**  
App should function consistently without interruption.

**Actual Result:**  
Customer reports app becomes unusable intermittently.

**Attachments / Logs:**  
None available yet.

**Next Action:**  
Collect additional details from customer and attempt reproduction under matching conditions.

---

### Follow-up Questions for Customer

1. What exactly happens when the app "doesn't work"?
   - Does it freeze, crash, fail to load, show an error, or stop responding?

2. Which phone model and OS version are you using?

3. Which app version are you currently on?

4. Approximately how often does the issue happen?

5. Is there any common action before the issue occurs?

6. Does restarting the app temporarily fix the issue?

7. Are you on Wi-Fi, mobile data, or both when this happens?

8. Can you share screenshots or screen recordings if it happens again?\


## Part 2 — Test Case Design

### Feature: Invite a team member by email

| ID | Test Case | Expected Result |
|---|---|---|
| TC-01 | Send invite with valid email | Invite sent successfully |
| TC-02 | Enter invalid email format | Validation error shown |
| TC-03 | Leave email field empty | Required field validation shown |
| TC-04 | Open invite link before expiry | User can join successfully |
| TC-05 | Open invite link after 48 hours | Link is marked expired |
| TC-06 | Send invite to existing team member | Proper error message shown |
| TC-07 | Click Send Invite multiple times quickly | Duplicate invites are prevented |
| TC-08 | Verify success message after sending invite | Confirmation message displayed |
| TC-09 | Verify behavior on server/API failure | Error message displayed |
| TC-10 | Verify invite link cannot be reused | Link becomes invalid after use |

### Edge Cases

- Email with leading/trailing spaces
- Uppercase email input
- Slow network during invite
- Invite email delayed
- Invite opened on different device/browser

## Part 3 — Exploratory Testing Observations

### Tested Accounts

- standard_user
- problem_user
- locked_out_user
- performance_glitch_user
- error_user
- visual_user

---

### standard_user

- Login credentials are working correctly
- Website is responsive across screens tested
- Add to cart functionality is working
- Only one item can be added/removed at a time from product card
- Sorting filters are working:
  - Name (A-Z, Z-A)
  - Price (low-high, high-low)
- Cart and checkout flow are working correctly
- "Reset App State" option in sidebar is not working

---

### problem_user

- All products are displaying the same image
- Only able to add one product to cart
- About page returns 404
- Sorting/filtering options are not working
- Product preview image differs from actual product image

---

### locked_out_user

- User is unable to log in
- Account appears intentionally locked

---

### performance_glitch_user

- Login takes noticeably longer than normal
- Sorting/filter actions are working but very slow
- "Reset App State" option is not working
- About page is working correctly

---

### error_user

- Products can be added to cart
- Remove from cart does not work directly from product card
- Item can only be removed from cart page
- Add to cart works only for a few products
- Sorting/filtering options are not working

---

### visual_user

- Filtering behavior appears visually inconsistent
- Product prices change unexpectedly during filtering
- Checkout flow is working
- About page is working
- Add/remove cart functionality works correctly from product card