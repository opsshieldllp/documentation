---
title: Trial & Purchasing a License
---

To start a trial or purchase a new license, visit:
[https://manage.opsshield.com/order/main/index/cPGuard_Licenses](https://manage.opsshield.com/order/main/index/cPGuard_Licenses)

---

## Trial License

- The trial license has **no limitations** — all features available in the paid version are included
- The trial license will **expire automatically** at the end of its term
- To continue using cPGuard after the trial, you will need to purchase a new license and apply it to your server

---

## Purchase a Paid License

Once you are satisfied with the trial and ready to proceed with a paid plan, purchase a license from:
[https://manage.opsshield.com/order/main/index/cPGuard_Licenses](https://manage.opsshield.com/order/main/index/cPGuard_Licenses)

After completing the payment, you will receive a new license key. To activate it on your server, run the following command replacing `LICENSE-KEY` with your actual key:

```bash
cpgcli license --key LICENSE-KEY
```

---

## Switch from Trial to Standard License

There is no direct option to convert a trial license to a paid license. You need to:

1. Cancel the current trial license
2. Purchase a new Standard license
3. Apply the new license key on your server

> ⚠️ A canceled license cannot be restored.

---

## Upgrade from Standard to Unlimited License

There is no direct upgrade option available. You need to:

1. Purchase a new Unlimited license
2. Apply the new license key on your server
3. Cancel the old Standard license

> ⚠️ A canceled license cannot be restored.