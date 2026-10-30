# Labeling guide

Only incoming messages (`type=1`) are labeled. Each message gets exactly one label.

## Categories

| Label | Definition | Examples |
|---|---|---|
| OTP | One-time codes for login, payment or verification | "123456 is your OTP for…", Aadhaar OTP |
| Transaction | Money actually moved or a balance/holding was reported | debit/credit alerts, UPI sent/received, purchase receipts/e-bills, mutual fund purchase/SIP/redemption/allotment, NSE/BSE fund balance, CDSL/NSDL holding statements |
| Government | Sent by a government body, not an OTP | UIDAI, EPFO, Income Tax, traffic challan, election, disaster alerts, `-G` senders |
| Reminder | Time-bound action due from you | bill/EMI due, policy renewal, card expiry, appointment tomorrow, failed/pending payment |
| Service | Informational update about something you already use, no action required | order shipped/delivered, exchange/regulator advisories, KYC updated, ticket booked, plan activated |
| Promotional | Legitimate marketing from a registered business | offers, pre-approved loans, sales, cashback campaigns |
| Spam | Deceptive wording, fake urgency, impersonation or malicious links | "your account will be blocked, click…", lottery wins, job scams |
| Personal | Written by a human to you, or a notice about a human trying to reach you | friends, family, delivery agent texting directly, missed-call alerts |
| Unknown | Doesn't fit any of the above | |

## Precedence (first match wins)

1. **Spam**: deception beats everything. A fake "Rs.5000 debited, click here to dispute" is Spam, not Transaction.
2. **OTP**: a bank or government OTP is still OTP.
3. **Transaction**
4. **Government**
5. **Reminder**
6. **Service**
7. **Promotional**
8. **Personal**
9. **Unknown**

## Edge cases

These override the precedence list.

- The same loan offer is **Promotional** from a `-P` DLT sender but **Spam** from a 10-digit number or with an unofficial shortened link.
- Payment failed, not completed or pending (no money moved) → **Reminder**.
- Awareness/fraud advisories from exchanges, regulators or banks ("beware of unsolicited tips…") → **Service**, not Spam.
- Investment confirmations ("thank you for your investment in … fund of Rs. …") → **Transaction**, not Service.
- Missed-call alerts ("you have a missed call from …") → **Personal**.
- "Your statement is ready" → **Service**. "Payment due on 05/11" → **Reminder**.
- Thank-you/e-bill/receipt after a purchase → **Transaction**.
- A utility bill generated with a due date → **Reminder**.
- A message written by a human on behalf of a business (e.g. a broker's personal number) → **Personal**.
