# Google Review Request (Lite): free n8n workflow for Australian businesses

When a job is done, the customer gets one friendly SMS with your Google review link, a couple of hours later and inside your sending hours.

- Webhook in (`/webhook/review-lite/job-complete`) → one SMS via Twilio after `delayHours`
- AU number normalisation (04xx → +614xx); only texts mobiles, skips landlines
- Weekday sending window in your timezone; never asks the same number twice within 180 days
- STOP/START opt-out handling (Spam Act 2003: every text has your business name and "Reply STOP to opt out")
- No review gating: everyone gets the same honest ask (Google policy / ACCC)
- Daily SMS cap and optional webhook secret (`sharedSecret`)

Tested end-to-end in n8n with simulated APIs (a local mock of the Twilio API); no live SMS or email provider was used in testing.

## Setup (about 15 minutes)
1. n8n → **Import from file** → `workflow/google-review-request-lite.n8n.json`.
2. Open **Decide** and set `businessName`, `googleReviewUrl` (Google Business Profile → Ask for reviews), `sms.from` (your Twilio AU mobile in +61 format) and `sms.twilioAccountSid`.
3. On **Send SMS (Twilio)**, add an HTTP Basic Auth credential: User = Account SID, Password = Auth Token.
4. Activate. Set your Twilio number's **Messaging** webhook to `https://<your-n8n>/webhook/review-lite/sms-inbound` (for STOP replies).
5. When a job is finished, POST `{"name":"Jo","phone":"0491 570 006"}` to `https://<your-n8n>/webhook/review-lite/job-complete` (from your job app, Zapier/Make, or curl).

Costs (Twilio, Oct 2026): about US$0.0515 (≈A$0.074) per SMS and US$8.25/month (≈A$11.90) for an AU mobile number.

## Want more?
The **Pro pack** adds:
- email as well as SMS (with one-click unsubscribe), or email-only
- click tracking and **one** reminder only for customers who didn't click
- customer replies forwarded to your mobile (catch complaints before they become 1-star reviews)
- Saturday hours, public holidays and Christmas closedown
- ClickSend support, plus dry-run mode
- duplicate-job protection by job reference, a per-run cap for big imports, and a status page with click rate
- an AU compliance guide (Spam Act, Google policy, ACCC), setup docs and test scripts

**Want the full Australian version?** The Google Review Request Pack (Pro) adds email, click tracking, a single smart reminder, reply forwarding, holidays, ClickSend and full setup docs. Store link coming soon; questions to aussieflows@mail.grokbot.com.

[Notify me when Pro is out](mailto:aussieflows@mail.grokbot.com?subject=Notify%20me%20-%20Google%20Review%20Request%20Pack).

Rather not set this up yourself? A done-for-you managed service (we set up, monitor and maintain these automations for your business; AU, from A$99/month) is being considered. [Join the waitlist](mailto:aussieflows@mail.grokbot.com?subject=Hosted%20waitlist%20-%20Google%20Review%20Request%20Pack). One email if it goes ahead or doesn't, then your address is deleted.

## More free AU workflows
Other free n8n workflows for Australian small businesses (MIT, same setup style):
- [Missed-Call Text-Back](https://github.com/fletcheri/n8n-missed-call-textback): text back missed callers in seconds
- [Quote Follow-Up](https://github.com/fletcheri/n8n-quote-follow-up): a polite follow-up SMS after you send a quote
- [Booking Reminder](https://github.com/fletcheri/n8n-booking-reminder): a reminder SMS before each booking
- [Overdue Invoice Reminder](https://github.com/fletcheri/n8n-overdue-invoice-reminder): an overdue-invoice email a week after the due date

MIT licence. Not legal advice.
