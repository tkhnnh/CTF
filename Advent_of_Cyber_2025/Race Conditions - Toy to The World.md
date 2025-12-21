A race condition happens when two or more actions occur at the same time, and the system’s outcome depends on thebunny character showing car racing. order in which they finish.

# Types of Race Conditions
- Time-of-Check to Time-of-Use (TOCTOU) -> a program checks something first and uses it later, but the data changes in between. For example, two users buy the same "last item" at the same time because the stock was checked before it was updated.
- Shared resource -> multiple users or systems try to change the same data simultaneously without proper control
- Atomicity violation -> For example, a payment is recorded, but the order confirmation fails because another request interrupts the process.
