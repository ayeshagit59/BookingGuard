# Booking Guard

**A concept AI agent that protects independent hotels from no-shows.**

**[Try the clickable prototype](https://ayeshagit59.github.io/BookingGuard/)** · **[Read the product note (PDF)](docs/Booking_Guard_Product_Note.pdf)**

<!-- Add a screenshot or GIF here: ![Booking Guard](docs/screenshot.png) -->

Booking Guard is a concept AI agent I designed for mid-sized independent Indian hotels that already collect payments online through payment links. For every direct booking that is paid at the hotel, it decides whether to ask for an advance, how much, and by when. When an unpaid room should go back on sale, it recommends a release, and a person makes the final call.

The prototype runs the agent on a fictional 40-room hotel, **Aster Stays**, with 99 days of simulated bookings calibrated to an operator interview and public benchmarks.

## The problem

About 30% of a mid-sized hotel's bookings are direct (phone, WhatsApp, walk-in, website) and paid at check-in. That is where no-shows concentrate: 5–10% overall, and almost none for prepaid bookings. Every no-show is a room that earns nothing, but asking everyone for an advance also loses guests. Front desks use one blunt rule, such as an advance from everyone in peak season and none off-peak. The right ask depends on the guest, the night and the timing, so no single rule fits.

## How it works

1. **Reads the booking.** Signals answer two questions: how likely is this guest not to come (*risk*), and how much is lost if they don't (*stakes*). The signals are the guest's history at this hotel, the booking channel, how far ahead it was booked, the guest's response, how full the night is, and the nights and rooms booked.
2. **Decides the ask.** It compares three options: a reconfirmation (a WhatsApp message with no payment), a 20% token advance, or the first night's room rent. It estimates the revenue likely to be lost under each, including guests who would cancel because they were asked to pay, and picks the lowest loss within the owner's limits, with a deadline.
3. **Acts on WhatsApp.** It sends the request with Pay, Confirm and Cancel buttons, reminds if there is no action, replaces failed payment links, and reads replies (including Hinglish) to judge whether the guest is coming.
4. **Asks a person.** If the deadline passes unpaid, it recommends whether to release the room and why. The manager decides.
5. **Learns.** It records each outcome and adjusts its rules within the owner's limits. Every change is shown to staff and can be undone. In one example, 5 in 100 last-minute guests on medium-demand nights cancelled when asked for a token, so it switched to a reconfirmation and cancellations fell to 1.5 in 100.

## What is controlled by people and by fixed rules

The owner sets the limits: the maximum advance, who approves releases, quiet hours, and guests to always trust or flag. Decisions that are too costly to get wrong are fixed rules, not AI:

- **No room is ever released without a person's approval.**
- **Trusted guests are never asked for money.** If one doesn't reply, the agent asks staff to call instead of suggesting a release.
- **No messages go out during quiet hours** (9:30 PM to 8:30 AM by default).
- **If the booking system stops syncing, the agent stops** and alerts staff.

## How success is measured

**North-star metric: revenue lost per 100 direct room-nights**, compared with a group of bookings that stays on today's process. It counts rooms lost to no-shows, guests who cancelled because they were asked to pay, and genuine guests released by mistake, so it can't be improved just by asking everyone for money.

Simulated results (99 days, 336 direct bookings, 1,000 runs):

| Metric | Today's process | With Booking Guard |
|---|---|---|
| Revenue lost per 100 direct room-nights (north star) | ₹34,577 | ₹29,450 (−15%) |
| No-show rate | 5.3% | 3.0% |
| Advance collected through payment links | ₹2.95 L | ₹3.48 L (+18%) |

That is about ₹1.6 lakh a year less revenue lost for one 40-room hotel.

**Guardrails, with an automatic response:**

- If guests who cancel after being asked to pay exceed 2% in a week for any booking type (1.7% in the simulation), the agent returns to the softer ask for that type and alerts the owner.
- If genuine guests released by mistake exceed 1% (0.7% in the simulation), every release suggestion needs a second staff check.

## Key product decisions

- **Only direct, pay-at-hotel room bookings, including long stays.** Prepaid OTA bookings carry no risk, OTAs don't let hotels take advances on pay-at-hotel bookings, and corporate credit accounts follow a separate billing flow.
- **The agent recommends releases instead of making them,** because releasing a guest who does arrive costs more than an empty room.
- **It asks less on quiet nights,** because a freed room rarely resells when demand is low.
- **Left out on purpose:** banquet advances (a separate flow, on the roadmap), dynamic pricing and no-show fees, and a general chatbot.

## How I would validate it

1. **Calibrate and review-first pilot (weeks 1–2):** 3–5 hotels, using 3–6 months of their real booking and payment records. Staff approve every request the agent sends.
2. **Live pilot (weeks 3–12):** the agent handles about 80% of bookings and the rest stay on today's process for comparison. A small test group keeps a token on quiet nights to measure whether paying makes guests likelier to come.
3. **Go / no-go:** roll out to more hotels if revenue lost per 100 room-nights falls by at least 10% and both guardrails hold.

The full reasoning, signal table and sources are in the [product note](docs/Booking_Guard_Product_Note.pdf).

## The prototype

Open the [live prototype](https://ayeshagit59.github.io/BookingGuard/), or download this repo and open `index.html` in a browser. There is no server or build step. It is a single self-contained HTML file and works best on a laptop screen.

**A good way to explore it:**

1. Sign in and take the short tour.
2. Open **Bookings** and click a direct booking to see why the agent made its decision.
3. Open **Needs you** and handle a decision. When the room is released, watch it get rebooked.
4. Choose **Watch one booking** from the account menu to follow one guest from first ask to payment.
5. Open the **Report** to see the north-star metric, the guardrails and what the agent learned.
6. Open **Settings** and change the owner's advance limit to see the agent's decisions change.

### What is real and what is simulated

The prototype is a clickable simulation of the product described in the note.

- All bookings, guests, hotel details and results are **synthetic**.
- WhatsApp messages, payment links and the owner's assistant are **scripted**. There are no live integrations.
- It runs entirely in the browser, with no server and no language model. The only external request is for fonts.
- Hotel and room images are AI-generated.

This is a concept project. It is not affiliated with or endorsed by any company.

## Sources

1. Interview with an independent hotel operator in India, October 2026.
2. CIO, Treebo Hotels, on payment links: higher no-shows on pay-at-hotel bookings, and 25–30% fewer after an advance via payment links.
3. HelloShift, "Hotel no-show": average rate 5–10%, prepaid near zero.

## About

Designed and prototyped by [Ayesha Ashraf Sultana](https://github.com/ayeshagit59). The wider portfolio is at [inkandash.vercel.app](https://inkandash.vercel.app).
