# PRD: Copy customer email

Status: implemented in pull request #1 on the fork (Dieguette/accounted)
Related: prototype/pad.md (product definition)

## Summary

The customer detail page shows the customer's email address. This feature adds a small copy button right after the address. One click copies the address to the clipboard and shows a confirmation message.

## Problem

On the customer detail page, the email address was a `mailto:` link and nothing else. Clicking it opens the user's mail program, which is often not what they want. A user who needs the address somewhere else (a quote, a chat message, another tool) had to select the text by hand and copy it.

## Goals

- Let a user copy a customer's email address with one click.
- Make it clear that the copy worked, or that it failed.
- Fit the existing look and behaviour of the app: no new visual patterns.

## Not goals

- Changing customer data or the email address itself.
- Sending emails from the app.
- Changing who can see or edit customers (permissions).
- Copy buttons for other fields (phone, address, organization number).
- A copy button on the supplier page or in the customer list.

## Who it is for

Anyone who can open a customer in the company. The button only reads and copies what is already on screen, so it is shown to every role, including users with read-only access.

## Where it appears

Invoicing > Customers > open a customer > "Contact details" section > "Email" row. The copy button is a small icon directly after the email address.

## Requirements

1. If the customer has an email address, a copy icon button appears right after the address.
2. Clicking the button copies exactly the address shown on the page.
3. After a successful copy, a confirmation message appears: "Email address copied" (Swedish: "E-postadressen kopierades").
4. If the copy fails (for example the browser blocks clipboard access), an error message appears: "Could not copy the email address" (Swedish: "Kunde inte kopiera e-postadressen"). The address stays visible so the user can select it by hand.
5. If the customer has no email address, the row shows the usual dash and there is no copy button.
6. The email address stays a normal link that opens the mail program. The copy button does not replace it.
7. The button has an accessible label and a hover tooltip: "Copy email address" (Swedish: "Kopiera e-postadressen").
8. The text is available in both Swedish and English, like all text in the app.

## Behaviour details

- Messages appear as the app's standard pop-up notification in the top right corner, the same place as all other confirmations in the app.
- Clicking the button again copies again and shows the message again.
- Copying does not change any data and does not call the server.
- Clipboard access needs a secure page (https, or localhost during development). On an insecure page the browser can refuse, and the error message is shown.

## Technical notes

- One page changed: the customer detail page, plus the Swedish and English text files.
- Reuses the app's existing clipboard helper and notification system. No new dependencies.
- No database, API or accounting changes. No migrations.

## Acceptance checks

1. The copied text matches the displayed email address.
2. The success message appears after clicking.
3. A customer without an email address shows no copy button.
4. A failed copy shows the error message.
5. The button works the same in Swedish and English, with translated texts.

## Verified so far

- Lint, type check and the repository's guard checks pass.
- Tested by hand on a local copy: the icon appears after the address, copying works and the confirmation message appears.
- Not tested: the failure message (hard to trigger on localhost) and a customer without an email address.

## Open questions

- Should the confirmation appear at the bottom centre of the screen? The app shows all notifications in the top right, and moving them would change every notification, so it is not part of this feature.
- Should the supplier page get the same button? Its email is currently plain text.

## Possible follow-up change (for the update test)

Also copy the phone number from the same section, with the same behaviour and messages.
