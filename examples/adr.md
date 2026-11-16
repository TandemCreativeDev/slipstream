# ADR-009: Refunds go to the original payment method

Status: Accepted
Date: 2026-11-16

## Context

Guests can now cancel a booking and are refunded. The payment provider can return money to the original payment method or issue a voucher. The client confirmed a refund window of 14 days, so the money has to be returned in cash terms inside that window.

## Decision

Refund to the original payment method through the provider's refund call. Vouchers are not issued.

## Alternatives

- Issue a voucher: instant and free, but the client's contract promises money back, and a voucher does not meet that.
- Let the guest choose: doubles the paths to build and test, for no stated need.

## Consequences

- Easier: the refund shown to the guest always matches what the provider returns.
- Harder: bookings paid by voucher have no original method to refund to and need their own rule.
- Impossible: refunding after the provider's own refund limit, which is outside our control.
