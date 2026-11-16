# Refund payment

Story: Guests can cancel a booking

Add the endpoint that cancels a booking and returns the refund amount.

## Depends on

- Bookings store (this story)
- Sign-in session (story: Guests can sign in)

## Proves

- Criterion 2: A guest cannot cancel someone else's booking
- Criterion 3: Staff can still cancel any booking

## Notes

- Follow the shape of the existing booking routes
- Reuse the existing access roles: guest and staff

## Done when

- The migration applies cleanly to a fresh database `cli`
