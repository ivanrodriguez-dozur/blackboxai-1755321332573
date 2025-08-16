```markdown
# Implementation Plan for UI Adjustments and Dark Mode Fix

Below is the detailed step-by-step plan covering every dependent file change, error handling, and best practices required.

---

## 1. Extend Banner Transition Interval
- **File:** src/components/common/PromoBanner.tsx  
  - Locate the timer/interval setting for banner color changes.
  - Change the interval value from its current value (e.g., 5000–10000 ms) to 30000 ms.
  - Retain the current color combinations and transition effects.
  - Wrap the timer logic in error handling (try/catch) to avoid runtime issues.

- **File (if applicable):** styles.css or src/app/globals.css  
  - Verify that no CSS transition duration conflicts exist.  
  - Adjust any CSS variables if used for timing.

---

## 2. Update Header Top Bar
- **File:** src/components/layout/Header.tsx  
  - **Remove Elements:**  
    - Delete the dark mode toggle button code.
    - Remove the hamburger menu icon (three bars).  
  - **Modify Icons:**  
    - Replace the notifications bell icon with a text button labeled “Notificaciones” styled with modern typography and spacing.
    - Replace the magnifying glass (search) icon with a text button labeled “Buscar”; ensure clear, readable labels.
  - Add proper accessibility attributes (e.g., aria-label) and error fallback logic if the UI element fails to render.

---

## 3. Fix Bottom Navigation
- **File:** src/components/layout/BottomNavigation.tsx  
  - **Remove Elements:**  
    - Delete the magnifying glass element.
    - Delete the five small dots element.
  - **UI Behavior:**  
    - Modify the component’s container or CSS class to use `position: fixed; bottom: 0; left: 0; right: 0; z-index: 9999;` so it remains in place regardless of the current view.
  - Ensure removal does not break the layout on different device sizes.

---

## 4. Repair Dark Mode on Configuración Page
- **File:** src/app/configuracion/page.tsx  
  - Locate the non-functioning dark mode button.
  - Connect its onClick handler to the theme toggle function provided by the ThemeContext.
  - Verify that toggling changes background, font color, and other UI elements so that text remains readable.
  - Add error handling (e.g., check if ThemeContext exists) and fallback behavior.

- **File:** src/contexts/ThemeContext.tsx  
  - Verify that the `toggleTheme` function updates the theme state correctly.
  - Confirm that all subscribed components (including the configuracion page) re-render with the correct theme changes.

---

## 5. Remove Duplicate Close Buttons in Modals
- **Files:**  
  - src/components/modals/NotificationsModal.tsx  
  - (Also check similar modals in favorites and any other pop-up windows such as src/app/favoritos/page.tsx if they incorporate modal elements)
  - Locate the rendering logic for the close ("x") button.
  - Remove any duplicate “x” so that only one close button is visible per modal.
  - Verify that the remaining button correctly triggers the modal close action and includes proper ARIA attributes.

---

## 6. Fix "Ver Todo" Button Duplication
- **File:** src/components/modals/NotificationsModal.tsx (or other modals displaying "Ver todo")  
  - Identify two instances of “Ver todo” (View All).
  - Remove the non-functioning duplicate, retaining only one “Ver todo” button.
  - Ensure the remaining button has a proper onClick handler (or correct link/navigation) so that it functions as intended.
  
---

## 7. Final Testing and Code Clean-Up
- Run the application locally and verify:
  - Banners now change every 30 seconds.
  - The header no longer contains the dark mode button or hamburger menu, and the notifications/search elements are clear text labels.
  - The bottom navigation is fixed and displays correctly.
  - The configuracion page dark mode toggle functions and alters font and background colors.
  - All modals (notifications, favorites, etc.) display only one close button and only one active “Ver todo” button.
- Review console logs for errors and add try/catch blocks as necessary.
- Confirm responsiveness and accessibility compliance.
  
---

## Summary
• Adjusted banner timing in PromoBanner.tsx to 30 seconds while preserving transitions.  
• Removed dark mode toggle and hamburger menu from Header.tsx, replacing icons with clear text labels.  
• Eliminated the magnifying glass and dots from BottomNavigation.tsx and fixed its position.  
• Linked the dark mode button in configuracion/page.tsx to ThemeContext.toggleTheme and ensured complete theme updates.  
• Removed duplicate close buttons and extra “Ver todo” in modals (NotificationsModal.tsx and favorites).  
• Validated all changes for responsiveness, error handling, and accessibility.
