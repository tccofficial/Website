# Contact Form Setup Guide

## Option 1: EmailJS (Recommended - No Backend Required)

### Step 1: Sign up for EmailJS
1. Go to [EmailJS.com](https://www.emailjs.com/)
2. Create a free account
3. Verify your email address

### Step 2: Set up Email Service
1. In EmailJS dashboard, go to "Email Services"
2. Click "Add New Service"
3. Choose your email provider (Gmail, Outlook, etc.)
4. Follow the authentication steps
5. Note down your **Service ID**

### Step 3: Create Email Template
1. Go to "Email Templates"
2. Click "Create New Template"
3. Use this template:

```html
Subject: New Contact Form Message from {{user_name}}

Name: {{user_name}}
Email: {{user_email}}
Message: {{message}}

This message was sent from the Trinity Computer Council website contact form.
```

4. Note down your **Template ID**

### Step 4: Get Your Public Key
1. Go to "Account" → "API Keys"
2. Copy your **Public Key**

### Step 5: Update the Code
Replace the placeholders in `script.js`:

```javascript
// Line 4: Replace YOUR_PUBLIC_KEY
emailjs.init("your_actual_public_key_here");

// Line 35: Replace YOUR_SERVICE_ID
emailjs.send('your_service_id_here', 'your_template_id_here', templateParams)
```

### Step 6: Test the Form
1. Open your website
2. Fill out the contact form
3. Submit and check if you receive the email

---

## Option 2: Formspree (Alternative - No Backend)

### Step 1: Sign up for Formspree
1. Go to [Formspree.io](https://formspree.io/)
2. Create a free account
3. Create a new form

### Step 2: Update the Form
Replace the form in `index.html`:

```html
<form action="https://formspree.io/f/YOUR_FORM_ID" method="POST" id="contactForm">
    <div class="form-group">
        <input type="text" name="name" placeholder="Your Name" required>
    </div>
    <div class="form-group">
        <input type="email" name="email" placeholder="Your Email" required>
    </div>
    <div class="form-group">
        <textarea name="message" placeholder="Your Message" rows="5" required></textarea>
    </div>
    <button type="submit" class="btn btn-primary">
        <span>Send Message</span>
        <i class="fas fa-paper-plane"></i>
    </button>
</form>
```

### Step 3: Update JavaScript
Replace the contact form code in `script.js`:

```javascript
// Contact form submission with Formspree
const contactForm = document.querySelector('#contactForm');
if (contactForm) {
    contactForm.addEventListener('submit', (e) => {
        // Formspree handles the submission automatically
        // We just need to show loading state
        
        const submitBtn = contactForm.querySelector('button[type="submit"]');
        const originalText = submitBtn.innerHTML;
        submitBtn.innerHTML = '<span>Sending...</span><i class="fas fa-spinner fa-spin"></i>';
        submitBtn.disabled = true;
        
        // Formspree will handle the rest
        // You can add success/error handling if needed
    });
}
```

---

## Option 3: Netlify Forms (If hosting on Netlify)

### Step 1: Update the Form
Add `netlify` attribute to your form:

```html
<form id="contactForm" netlify>
    <div class="form-group">
        <input type="text" name="name" placeholder="Your Name" required>
    </div>
    <div class="form-group">
        <input type="email" name="email" placeholder="Your Email" required>
    </div>
    <div class="form-group">
        <textarea name="message" placeholder="Your Message" rows="5" required></textarea>
    </div>
    <button type="submit" class="btn btn-primary">
        <span>Send Message</span>
        <i class="fas fa-paper-plane"></i>
    </button>
</form>
```

### Step 2: Deploy to Netlify
1. Push your code to GitHub
2. Connect your repository to Netlify
3. Deploy your site
4. Go to Netlify dashboard → Forms to see submissions

---

## Option 4: Custom Backend (Advanced)

If you have a server, you can create a PHP script:

### Create `contact.php`:
```php
<?php
if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $name = $_POST['user_name'];
    $email = $_POST['user_email'];
    $message = $_POST['message'];
    
    $to = "tcc@trinity.edu.np";
    $subject = "New Contact Form Message from $name";
    
    $email_content = "Name: $name\n";
    $email_content .= "Email: $email\n\n";
    $email_content .= "Message:\n$message";
    
    $headers = "From: $email";
    
    if (mail($to, $subject, $email_content, $headers)) {
        echo json_encode(["success" => true]);
    } else {
        echo json_encode(["success" => false]);
    }
}
?>
```

### Update JavaScript:
```javascript
// Contact form submission with custom backend
const contactForm = document.querySelector('#contactForm');
if (contactForm) {
    contactForm.addEventListener('submit', (e) => {
        e.preventDefault();
        
        const formData = new FormData(contactForm);
        
        fetch('contact.php', {
            method: 'POST',
            body: formData
        })
        .then(response => response.json())
        .then(data => {
            if (data.success) {
                showNotification('Thank you for your message! We will get back to you soon.', 'success');
                contactForm.reset();
            } else {
                showNotification('Sorry, there was an error sending your message. Please try again.', 'error');
            }
        })
        .catch(error => {
            showNotification('Sorry, there was an error sending your message. Please try again.', 'error');
        });
    });
}
```

---

## Recommended Setup

**For your use case, I recommend EmailJS** because:
- ✅ No backend required
- ✅ Works with static hosting
- ✅ Free tier available
- ✅ Reliable delivery
- ✅ Easy to set up
- ✅ Professional email templates

## Troubleshooting

### Common Issues:
1. **Emails not sending**: Check your EmailJS configuration
2. **Form not submitting**: Check browser console for errors
3. **Spam folder**: Check your spam folder for test emails
4. **CORS errors**: Make sure you're using the correct service ID

### Testing:
1. Always test with a real email address
2. Check both success and error scenarios
3. Test on different browsers
4. Test on mobile devices

---

**Need help?** Check the EmailJS documentation or contact their support team. 