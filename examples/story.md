# Guests can cancel a booking

Epic: Manage · Release 2 · Must · 1.5 days · Assignee: Sam

A guest can cancel their own booking from their account and is told what they will be refunded, so nobody has to email support.

## Requirements

| Need | Detail |
| ---- | ------ |
| Cancel | From the booking page, with a confirmation step |
| Refund shown | Amount stated before the guest confirms |
| Who can cancel | The guest who made the booking, and staff |

## Not included

- Email confirming the cancellation (its own Should story)
- Choosing a reason for cancelling (its own Could story)
- Changing the dates of a booking (Release 3)

## Acceptance criteria

1. A guest cancels their own booking and it shows as cancelled `browser`
2. A guest cannot cancel someone else's booking `api`
3. Staff can still cancel any booking `api`
4. The refund shown matches the amount refunded `db`

## Depends on

- Guests can sign in (Release 1)
- Guests can make a booking (Release 1)
- Refund window (input: the client)

## Blocked on

The client confirms the refund window, in days, before the release starts.
