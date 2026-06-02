# Email Form Setup Guide

This guide explains how to set up automatic email sending for the contact form with the following features:
- ✅ Auto-generates incident numbers (format: INC-YYYYMMDD-XXXXXX)
- ✅ Send message button disabled until all fields are valid
- ✅ Message minimum 20 characters requirement
- ✅ Screenshot upload capability
- ✅ Email validation
- ✅ Real-time form validation

## Quick Setup (Recommended: EmailJS)

### Option 1: Using EmailJS (Easiest - Free Tier Available)

1. **Create EmailJS Account**
   - Go to https://www.emailjs.com
   - Sign up for a free account

2. **Get Your Credentials**
   - Go to Dashboard → API Keys
   - Copy your **Public Key**

3. **Create Email Service**
   - Click "Add Service" → select "Gmail" (or your email provider)
   - Follow the authorization flow
   - Copy the **Service ID** (format: `service_xxxxx`)

4. **Create Email Template**
   - Go to Email Templates → Create New Template
   - Use Template ID: `template_contact_form`
   - Configure template with these variables:
     ```
     From: {{from_email}}
     Subject: [FitTimer {{message_type}}] Incident #{{incident_number}}
     
     ---
     Incident Number: {{incident_number}}
     Timestamp: {{timestamp}}
     
     Name: {{from_name}}
     Email: {{from_email}}
     Type: {{message_type}}
     
     Message:
     {{message_body}}
     ```

5. **Update index.html**
   - Open `index.html`
   - Find the JavaScript section with EmailJS setup
   - Replace these values:
     ```javascript
     const EMAILJS_SERVICE_ID = 'service_your_id_here';      // Your Service ID
     const EMAILJS_TEMPLATE_ID = 'template_contact_form';    // Keep as is
     const EMAILJS_PUBLIC_KEY = 'your_public_key_here';      // Your Public Key
     ```

### Option 2: Using Formspree (Alternative)

If EmailJS doesn't work, the form automatically falls back to Formspree:

1. **Create Formspree Account**
   - Go to https://formspree.io
   - Sign up for a free account

2. **Create New Form**
   - Click "Create" → "New Form"
   - Confirm the email where you want to receive submissions
   - Copy your **Form ID** (format: `f/xxxxx`)

3. **Update Fallback URL**
   - In `index.html`, find the fetch URL in the submitForm function:
   - Replace `'f/xyzabc'` with your Formspree Form ID

### Option 3: Using Your Own Backend (Advanced)

If you have your own backend, modify the `submitForm()` function:

```javascript
// Replace this section:
await fetch('https://formspree.io/f/YOUR_ID', { ... })

// With your own endpoint:
await fetch('https://your-api.com/send-email', { 
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    name, email, type, message: msg, incident: incidentNumber
  })
})
```

Your backend should:
- Accept the form data
- Send email to `developeranurag2108@gmail.com`
- Include the incident number in the email subject

## Features Implemented

### 1. **Button State Management**
- Button is **disabled** by default (appears grayed out)
- Activates only when:
  - Name field is filled
  - Valid email address is entered
  - Message is at least 20 characters
- Real-time validation as user types

### 2. **Incident Number Generation**
- Format: `INC-YYYYMMDD-XXXXXX`
- Example: `INC-20250602-A7K9B2`
- Unique for each submission
- Included in email subject and message

### 3. **Screenshot Upload**
- Optional file input (accept images only)
- Live preview of selected image
- File integrated into email (if backend supports)

### 4. **Form Validation**
- Email validation (basic regex)
- Character count for message
- Visual feedback on button state
- Clear error messages

### 5. **User Feedback**
- Loading state while sending
- Success message with incident number
- Error handling with fallback options
- Form auto-clears after successful submission

## Testing

### Local Testing
1. Fill in the form with valid data
2. Verify button is disabled/enabled correctly
3. Watch character counter update
4. Upload a screenshot (optional)
5. Click "Send Message"
6. Check your email for the submission

### Test Data
```
Name: Test User
Email: test@example.com
Type: Bug Report
Message: This is a test message to verify the form is working correctly.
```

## Email Format Received

When a message is sent, you'll receive an email like:

```
Subject: [FitTimer Bug Report] Incident #INC-20250602-A7K9B2

---
Incident Number: INC-20250602-A7K9B2
Timestamp: 6/2/2025, 3:00:00 PM

Name: Test User
Email: test@example.com
Type: Bug Report

Message:
This is a test message to verify the form is working correctly.
```

## Troubleshooting

### Button stays disabled
- Check all fields are filled
- Verify email format (must have @ and domain)
- Ensure message is at least 20 characters

### Email not received
1. Check spam/promotions folder
2. Verify Service ID and Public Key are correct (EmailJS)
3. Ensure email template variables match
4. Check Formspree Form ID (fallback option)
5. Check browser console for JavaScript errors (F12)

### Screenshot upload not showing
- Ensure file is a valid image format
- Check browser console for errors
- Verify file size isn't too large

### "Sending..." state never ends
- Check internet connection
- Verify credentials in code
- Check browser console (F12) → Network tab for failed requests

## GitHub Solutions Reference

This implementation uses popular GitHub-recommended solutions:

1. **EmailJS** - Client-side email service
   - GitHub: https://github.com/emailjs-com/emailjs-sdk
   - Pros: No backend needed, free tier, secure
   - Cons: Limited monthly free emails (200/month)

2. **Formspree** - Form backend service
   - Website: https://formspree.io
   - Pros: Simple setup, free tier, email forwarding
   - Cons: Limited features in free tier

3. **Nodemailer** - Node.js email library (if using backend)
   - GitHub: https://github.com/nodemailer/nodemailer
   - Pros: Full control, no limits
   - Cons: Requires backend infrastructure

## Security Considerations

- **Do NOT** hardcode sensitive credentials in production
- Use environment variables for API keys
- Validate all inputs on backend if using server
- Rate limit form submissions if using backend
- Store incident numbers for tracking purposes

## Customization

### Change incident number format
Modify `generateIncidentNumber()` function in the JavaScript section.

### Change minimum message length
Search for `>= 20` and replace with desired minimum length.

### Add more form fields
1. Add HTML input element in form
2. Add validation in `validateForm()` function
3. Add field to EmailJS template variables

### Change email recipient
- In email service: update recipient in template
- Keep `developeranurag2108@gmail.com` as primary or add as CC

## Support

For issues or questions:
- Email: developeranurag2108@gmail.com
- Check browser console (F12) for detailed error messages
- Verify all setup steps were completed correctly
