# Bug Reports — SauceDemo

Defects found during manual test execution of [saucedemo.com](https://www.saucedemo.com).
All defects were logged and tracked in Jira.

Full log: [`SauceDemo_Bug_Log.xlsx`](SauceDemo_Bug_Log.xlsx)

| ID | Summary | Module | Severity | Status |
|---|---|---|---|---|
| BUG-001 | Cart contents persist across user sessions after logout | Cart / Session | Critical | Open |
| BUG-002 | All product images render the same placeholder image | Products | High | Open |

---

## BUG-001 — Cart contents persist across user sessions after logout

**Severity:** Critical · **Priority:** High · **Module:** Cart / Session management

### Description

Items added to the cart by one user remain in the cart after an explicit logout and
are visible to the next user who logs in. Session data is not cleared on logout, so
one user inherits another user's cart contents.

This is a data isolation defect: each user account should have an independent cart.

### Steps to Reproduce

1. Navigate to https://www.saucedemo.com
2. Log in as `standard_user` / `secret_sauce`
3. Add **Sauce Labs Backpack** to the cart
4. Log out using the menu (not by closing the tab)
5. Log in as `problem_user` / `secret_sauce`
6. Open the cart

### Expected Result

The cart is empty. Logging out clears the previous user's session data, and each
account maintains its own cart.

### Actual Result

Sauce Labs Backpack ($29.99) is still in the cart, and the cart badge shows **1**.
The item carried over from the previous user's session.

### Evidence

![Cart retains previous user's item after logout](../Screenshots/01_cart_persists_across_users.png)

**Environment:** Chrome, Windows 11

---

## BUG-002 — All product images render the same placeholder image

**Severity:** High · **Priority:** High · **Module:** Products / Inventory

### Description

On the Products page, every product tile renders an identical image — a photograph of
a dog — regardless of the product it represents. This affects all six products
(Sauce Labs Backpack, Bike Light, Bolt T-Shirt, Fleece Jacket, Onesie, and
Test.allTheThings() T-Shirt). Product names, descriptions and prices render correctly;
only the images are wrong.

The issue is user-specific: the same page displays the correct product images when
logged in as `standard_user`, which indicates the defect is tied to the user account
rather than to the image assets themselves.

### Steps to Reproduce

1. Navigate to https://www.saucedemo.com
2. Log in as `problem_user` / `secret_sauce`
3. Observe the product images on the Products page

### Expected Result

Each product displays its own distinct image, matching the product name and description.

### Actual Result

All products display the same placeholder image. No error message or broken-image icon
is shown — the images load successfully, but they are the wrong asset.

### Evidence

![All products showing the same image](../Screenshots/02_problem_user_duplicate_images.png)

**Environment:** Chrome, Windows 11

---

## Severity Guide

| Severity | Meaning |
|---|---|
| Critical | Data integrity or security is affected, or a core flow is blocked |
| High | Major function broken or clearly incorrect, no workaround |
| Medium | Function works but behaves incorrectly in some cases |
| Low | Cosmetic, wording, or minor UI issue |
