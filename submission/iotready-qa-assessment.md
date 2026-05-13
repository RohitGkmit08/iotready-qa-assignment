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


## Part 4 — Investigation Scenario

### Initial Observation

- Issue happens only on mobile app
- Web dashboard continues showing live data
- Issue starts after app stays in background for some time
- Restarting the app fixes the problem temporarily

This suggests the backend is likely working correctly and the issue may be related to the mobile app lifecycle, caching, polling, or websocket reconnection.

---

### Investigation Plan

#### 1. Reproduce the issue consistently

Steps:
- Open mobile app and monitor live sensor readings
- Send app to background for a few minutes
- Trigger new sensor events from backend tool
- Compare mobile app data vs web dashboard data

Expected evidence:
- Mobile app stops updating while web app continues receiving live data

---

#### 2. Check whether API/WebSocket requests stop after backgrounding

Using DevTools/logs:
- Verify whether polling requests continue after app resumes
- Verify websocket connection status after returning from background

Possible confirmation:
- No new requests after app resumes
- Websocket connection remains disconnected/stale

Possible conclusion:
- App is failing to reconnect or restart polling after background state

---

#### 3. Check whether stale data is coming from local cache

Steps:
- Compare timestamps of sensor readings
- Force refresh data manually if possible

Possible confirmation:
- Cached data remains visible until app restart
- Fresh backend data exists but UI is not updating

Possible conclusion:
- Cache invalidation or state refresh issue

---

#### 4. Verify whether issue happens on multiple devices/platforms

Steps:
- Test on different OS versions/devices
- Compare Android vs iOS behavior if available

Possible confirmation:
- Issue isolated to one platform/device type

Possible conclusion:
- OS-specific background lifecycle handling issue

---

#### 5. Check app behavior during network reconnect

Steps:
- Background app
- Toggle network/Wi-Fi
- Resume app and trigger new events

Possible confirmation:
- App does not recover automatically after reconnect

Possible conclusion:
- Missing reconnection handling after network interruption

---

### Most Likely Root Cause

Most likely issue is that the mobile app stops receiving live updates after being backgrounded and does not properly reconnect/restart data synchronization when resumed.


## Part 5 — One Paragraph

One project I’m genuinely proud of testing was my Jira-style admin application built with React. I wrote both unit and integration tests using React Testing Library and Vitest, mainly around authentication and project management flows. I tested scenarios like login validation, failed and successful authentication, protected navigation, auth token storage, retry behavior after failed login, project creation/edit flows, API failure handling, modal interactions, and form validation states. I also mocked API layers and routing behavior to isolate components properly and verify user-facing behavior instead of implementation details. Working on these tests helped me improve how I think about real user flows, edge cases, and frontend reliability.