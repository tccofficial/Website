# 📧 Contact Form Setup Guide - Trinity Computer Council

## 🚀 EmailJS Integration (Recommended)

### **Step 1: EmailJS Account Setup**
1. Go to [EmailJS.com](https://www.emailjs.com/)
2. Create a free account
3. Verify your email address
4. Complete profile setup

### **Step 2: Email Service Configuration**
1. In EmailJS dashboard, go to **"Email Services"**
2. Click **"Add New Service"**
3. Choose your email provider:
   - **Gmail** (Recommended)
   - **Outlook**
   - **Yahoo**
   - **Custom SMTP**
4. Follow authentication steps
5. Note down your **Service ID**

### **Step 3: Email Template Creation**
1. Go to **"Email Templates"**
2. Click **"Create New Template"**
3. Use this template:

```html
Subject: New Contact Form Message from {{user_name}}

Name: {{user_name}}
Email: {{user_email}}
Message: {{message}}
Timestamp: {{timestamp}}

This message was sent from the Trinity Computer Council website contact form.
```

4. Note down your **Template ID**

### **Step 4: Get Your Public Key**
1. Go to **"Account"** → **"API Keys"**
2. Copy your **Public Key**

### **Step 5: Update Website Configuration**
Replace the placeholders in `script.js`:

```javascript
// Replace these values with your actual EmailJS credentials
emailjs.init("YOUR_PUBLIC_KEY");
const serviceId = "YOUR_SERVICE_ID";
const templateId = "YOUR_TEMPLATE_ID";
```

## 📧 Alternative Solutions

### **Option 2: Formspree (No Setup Required)**
1. Go to [Formspree.io](https://formspree.io/)
2. Create account
3. Create new form
4. Get form endpoint
5. Update form action in HTML

### **Option 3: Netlify Forms (If Using Netlify)**
1. Add `netlify` attribute to form
2. Forms automatically work
3. Access submissions in Netlify dashboard

### **Option 4: Google Forms Integration**
1. Create Google Form
2. Get form embed code
3. Replace contact form with Google Form

## 🔧 Advanced Configuration

### **Email Template Customization**
Create beautiful HTML email templates:

```html
<!DOCTYPE html>
<html>
<head>
    <style>
        body { font-family: Arial, sans-serif; }
        .header { background: #00d4ff; color: white; padding: 20px; }
        .content { padding: 20px; }
        .footer { background: #f5f5f5; padding: 10px; text-align: center; }
    </style>
</head>
<body>
    <div class="header">
        <h2>New Contact Form Message</h2>
    </div>
    <div class="content">
        <p><strong>Name:</strong> {{user_name}}</p>
        <p><strong>Email:</strong> {{user_email}}</p>
        <p><strong>Message:</strong></p>
        <p>{{message}}</p>
    </div>
    <div class="footer">
        <p>Trinity Computer Council - {{timestamp}}</p>
    </div>
</body>
</html>
```

### **Form Validation Enhancement**
Add client-side validation:

```javascript
// Enhanced validation
function validateForm() {
    const name = document.getElementById('user_name').value.trim();
    const email = document.getElementById('user_email').value.trim();
    const message = document.getElementById('message').value.trim();
    
    if (name.length < 2) {
        showError('Name must be at least 2 characters long');
        return false;
    }
    
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    if (!emailRegex.test(email)) {
        showError('Please enter a valid email address');
        return false;
    }
    
    if (message.length < 10) {
        showError('Message must be at least 10 characters long');
        return false;
    }
    
    return true;
}
```

### **Success/Error Handling**
Improve user experience:

```javascript
function showSuccess(message) {
    const notification = document.createElement('div');
    notification.className = 'notification success';
    notification.innerHTML = `
        <i class="fas fa-check-circle"></i>
        <span>${message}</span>
    `;
    document.body.appendChild(notification);
    
    setTimeout(() => {
        notification.remove();
    }, 5000);
}

function showError(message) {
    const notification = document.createElement('div');
    notification.className = 'notification error';
    notification.innerHTML = `
        <i class="fas fa-exclamation-circle"></i>
        <span>${message}</span>
    `;
    document.body.appendChild(notification);
    
    setTimeout(() => {
        notification.remove();
    }, 5000);
}
```

## 🎨 Styling Notifications
Add CSS for better notifications:

```css
.notification {
    position: fixed;
    top: 20px;
    right: 20px;
    padding: 15px 20px;
    border-radius: 8px;
    color: white;
    font-weight: 500;
    z-index: 1000;
    animation: slideIn 0.3s ease;
    display: flex;
    align-items: center;
    gap: 10px;
}

.notification.success {
    background: linear-gradient(135deg, #4CAF50, #45a049);
}

.notification.error {
    background: linear-gradient(135deg, #f44336, #d32f2f);
}

@keyframes slideIn {
    from {
        transform: translateX(100%);
        opacity: 0;
    }
    to {
        transform: translateX(0);
        opacity: 1;
    }
}
```

## 📊 Analytics Integration

### **Track Form Submissions**
Add Google Analytics tracking:

```javascript
// Track successful form submissions
function trackFormSubmission() {
    if (typeof gtag !== 'undefined') {
        gtag('event', 'form_submit', {
            'event_category': 'contact',
            'event_label': 'contact_form'
        });
    }
}
```

### **Conversion Tracking**
Set up goals in Google Analytics:
1. Go to Google Analytics
2. Admin → Goals
3. Create new goal
4. Set destination as thank you page
5. Track form submissions

## 🔒 Security Considerations

### **Rate Limiting**
Prevent spam submissions:

```javascript
let lastSubmissionTime = 0;
const SUBMISSION_COOLDOWN = 30000; // 30 seconds

function canSubmit() {
    const now = Date.now();
    if (now - lastSubmissionTime < SUBMISSION_COOLDOWN) {
        return false;
    }
    lastSubmissionTime = now;
    return true;
}
```

### **Honeypot Protection**
Add hidden field to catch bots:

```html
<input type="text" name="website" style="display: none;" tabindex="-1">
```

```javascript
// Check for honeypot
if (formData.get('website')) {
    // Bot detected, don't submit
    return;
}
```

## 📱 Mobile Optimization

### **Touch-Friendly Design**
Ensure form works well on mobile:

```css
.contact-form input,
.contact-form textarea {
    font-size: 16px; /* Prevents zoom on iOS */
    padding: 12px;
    border-radius: 8px;
}

.contact-form button {
    min-height: 44px; /* Touch target size */
    padding: 12px 24px;
}
```

## 🚀 Testing Checklist

### **Before Going Live**
- [ ] Test form submission
- [ ] Verify email delivery
- [ ] Check mobile responsiveness
- [ ] Test validation messages
- [ ] Verify analytics tracking
- [ ] Test error handling
- [ ] Check spam protection

### **Regular Maintenance**
- [ ] Monitor form submissions
- [ ] Check for spam
- [ ] Update email templates
- [ ] Review analytics data
- [ ] Test form functionality

---

**Need Help?** Contact the development team or refer to EmailJS documentation. 