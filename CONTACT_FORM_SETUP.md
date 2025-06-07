# Contact Form Setup Guide

## 🚀 Option 1: EmailJS (Recommended for Static Sites)

EmailJS is perfect for static hosting without a backend. **Free tier: 200 emails/month**

### Setup Steps:

1. **Create EmailJS Account**
   - Go to [emailjs.com](https://www.emailjs.com/)
   - Sign up for free account

2. **Setup Email Service**
   - Add your email service (Gmail, Outlook, etc.)
   - Follow the connection wizard

3. **Create Email Template**
   - Create a new template with these variables:
   ```
   Name: {{name}}
   Email: {{email}}
   Company: {{company}}
   Message: {{message}}
   ```

4. **Update the HTML**
   - Replace `YOUR_PUBLIC_KEY` with your EmailJS public key
   - Replace `YOUR_SERVICE_ID` with your service ID
   - Replace `YOUR_TEMPLATE_ID` with your template ID

### Example EmailJS Setup:
```javascript
emailjs.init("user_1234567890"); // Your public key
emailjs.sendForm('service_gmail', 'template_contact', form)
```

---

## 🔧 Option 2: Formspree (Simple Alternative)

Formspree handles forms without backend code. **Free tier: 50 submissions/month**

### Setup:
1. Go to [formspree.io](https://formspree.io/)
2. Create account and get form endpoint
3. Update form action:

```html
<form action="https://formspree.io/f/YOUR_FORM_ID" method="POST">
```

---

## 🛠️ Option 3: Backend with SMTP

If you want your own server, here's a Node.js example:

### Backend Code (server.js):
```javascript
const express = require('express');
const nodemailer = require('nodemailer');
const cors = require('cors');

const app = express();
app.use(cors());
app.use(express.json());

const transporter = nodemailer.createTransporter({
  service: 'gmail',
  auth: {
    user: 'your-email@gmail.com',
    pass: 'your-app-password'
  }
});

app.post('/contact', async (req, res) => {
  const { name, email, company, message } = req.body;
  
  const mailOptions = {
    from: email,
    to: 'hello@appcentercodepushpro.com',
    subject: `New Contact from ${name}`,
    html: `
      <h3>New Contact Form Submission</h3>
      <p><strong>Name:</strong> ${name}</p>
      <p><strong>Email:</strong> ${email}</p>
      <p><strong>Company:</strong> ${company}</p>
      <p><strong>Message:</strong> ${message}</p>
    `
  };
  
  try {
    await transporter.sendMail(mailOptions);
    res.json({ success: true });
  } catch (error) {
    res.status(500).json({ error: 'Failed to send email' });
  }
});

app.listen(3000);
```

### Frontend Update:
```javascript
fetch('/contact', {
  method: 'POST',
  headers: {'Content-Type': 'application/json'},
  body: JSON.stringify(formData)
});
```

---

## 🌐 Option 4: Serverless Functions

### Netlify Functions:
```javascript
// netlify/functions/contact.js
const nodemailer = require('nodemailer');

exports.handler = async (event) => {
  // Email sending logic here
  return {
    statusCode: 200,
    body: JSON.stringify({ message: 'Email sent' })
  };
};
```

### Vercel Functions:
```javascript
// api/contact.js
export default async function handler(req, res) {
  // Email sending logic here
  res.status(200).json({ message: 'Email sent' });
}
```

---

## 📧 Recommended Setup for Your Use Case:

**For Static Hosting (GitHub Pages, Netlify, Vercel):**
- Use **EmailJS** - it's free, reliable, and requires no backend

**For WordPress/Existing Backend:**
- Use **Contact Form 7** plugin or your existing contact form system

**For Custom Backend:**
- Use **Node.js + Nodemailer** for full control

---

## 🔐 Security Tips:

1. **Never expose SMTP credentials** in frontend code
2. **Use environment variables** for sensitive data
3. **Add rate limiting** to prevent spam
4. **Validate all inputs** on both frontend and backend
5. **Use CAPTCHA** for additional spam protection

---

## 📊 Cost Comparison:

| Service | Free Tier | Paid Plans |
|---------|-----------|------------|
| EmailJS | 200 emails/month | $15/month for 1000 |
| Formspree | 50 submissions/month | $10/month for 1000 |
| SendGrid | 100 emails/day | $15/month for 40k |
| AWS SES | 62,000 emails/month | Pay per email |

---

## ✅ Quick Start with EmailJS:

1. Sign up at emailjs.com
2. Connect your Gmail
3. Create template
4. Copy your IDs into the HTML
5. Test the form!

The form is already set up in your HTML - you just need to add your EmailJS credentials! 