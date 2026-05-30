# Form Porn Email Setup Guide
## hello@formporn.app · support@formporn.app · ceo@formporn.app

---

## STEP 1 — Cloudflare Email Routing (Free, 15 min)

This routes any @formporn.app email to your personal Gmail.
**You don't need a mail server. Completely free.**

### Prerequisites
- Your domain (formporn.app) must be on Cloudflare DNS
- If it's on Vercel, you'll add Cloudflare just for email — your site stays on Vercel

### Setup

1. Go to **cloudflare.com** → log in → click on `formporn.app`
2. In the left sidebar click **Email** → **Email Routing**
3. Click **Enable Email Routing**
4. Cloudflare will show you MX records to add — click **Add Records Automatically**
5. Under **Custom Addresses** click **Create Address**:

| Create this address | Forward to |
|---------------------|------------|
| `hello@formporn.app` | your-gmail@gmail.com |
| `support@formporn.app` | your-gmail@gmail.com |
| `ceo@formporn.app` | your-gmail@gmail.com |
| `press@formporn.app` | your-gmail@gmail.com |

6. Each address — enter it, enter your Gmail, click **Save**
7. Gmail will send a verification email — click the link in each one
8. Done. Emails sent to any @formporn.app address now land in your Gmail inbox.

### How it looks in Gmail
- Arrives labeled from: `hello@formporn.app via cloudflare.net`
- You can reply **as** hello@formporn.app using Gmail's "Send mail as" feature (below)

---

## STEP 2 — Reply as @formporn.app from Gmail

So when you reply, it shows **from ceo@formporn.app** not your Gmail.

1. In Gmail → **Settings (gear)** → **See all settings**
2. Click **Accounts and Import** tab
3. Under "Send mail as" → click **Add another email address**
4. Enter:
   - Name: `Form Porn`
   - Email: `ceo@formporn.app`
   - Uncheck "Treat as alias"
5. Click **Next Step** → **Send Verification**
6. Check your Gmail for the code → enter it
7. Repeat for `hello@formporn.app` and `support@formporn.app`

Now in Gmail's compose window you can choose which address to send from.

---

## STEP 3 — Contact Form → Your Email (via Resend)

This makes your contact.html form actually send you emails.

### Option A: Resend (Recommended — free tier = 3,000 emails/month)

1. Go to **resend.com** → Sign Up (free)
2. Click **Domains** → **Add Domain** → enter `formporn.app`
3. Resend gives you DNS records — add them in Cloudflare
4. Click **API Keys** → **Create API Key** → copy it
5. Add this to your contact form's submit handler:

```javascript
// In your contact.html submit button handler:
async function sendContactForm() {
  const name = document.getElementById('contact-name').value;
  const email = document.getElementById('contact-email').value;
  const message = document.getElementById('contact-message').value;

  const response = await fetch('https://api.resend.com/emails', {
    method: 'POST',
    headers: {
      'Authorization': 'Bearer YOUR_RESEND_API_KEY',
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      from: 'Form Porn Contact <hello@formporn.app>',
      to: ['ceo@formporn.app'],  // ← your email
      subject: `New Contact: ${name}`,
      html: `
        <h2>New Form Porn Contact</h2>
        <p><strong>Name:</strong> ${name}</p>
        <p><strong>Email:</strong> ${email}</p>
        <p><strong>Message:</strong></p>
        <p>${message}</p>
      `
    })
  });

  if (response.ok) {
    // Show success message
    document.getElementById('contact-success').style.display = 'block';
  }
}
```

### Option B: Formspree (Even simpler — no code, free tier)

1. Go to **formspree.io** → Sign Up
2. Click **New Form** → name it "Form Porn Contact"
3. Copy your form endpoint: `https://formspree.io/f/XXXXXXXX`
4. In your `contact.html`, change the form tag to:
```html
<form action="https://formspree.io/f/XXXXXXXX" method="POST">
  <input type="hidden" name="_subject" value="New Form Porn Contact">
  <input type="text" name="name" placeholder="Your Name">
  <input type="email" name="email" placeholder="Your Email">
  <textarea name="message" placeholder="Message"></textarea>
  <button type="submit">Send</button>
</form>
```
5. Done — submissions go straight to your email. No API key needed.

---

## STEP 4 — Professional Email Signatures

When replying from ceo@formporn.app in Gmail, use this signature:

```
Trei Alexander
CEO & Founder, Form Porn LLC
ceo@formporn.app
formporn.app

"She's Not Paper. She's The Proof."
```

---

## Summary

| What | Tool | Cost |
|------|------|------|
| Receive email at @formporn.app | Cloudflare Email Routing | Free |
| Reply as @formporn.app | Gmail "Send as" | Free |
| Contact form submissions | Resend or Formspree | Free tier |
| Transactional emails (receipts, etc.) | Resend | Free up to 3k/mo |

Total cost: **$0**
Setup time: **~20 minutes**
