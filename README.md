# KimTech Logistics Website

A single-file, responsive marketing site for a freight and logistics company. It is plain HTML, Tailwind CSS and vanilla JavaScript, with no build step needed to get started.

**File:** `kimtech-logistics.html`

## Features

- Responsive layout for mobile, tablet and desktop
- Light and dark mode with a header toggle (follows the device setting first, then remembers the visitor's choice)
- Sections: hero, global reach, services, why choose us, shipment tracking, about, testimonials, quote call to action, news, contact form and footer
- **Quote form** (modal) opened from "Get a Quote", "Book Now" and "Request a custom proposal". "Book Now" pre-selects the matching service.
- **Contact form** (section) with name, email, optional phone, topic and message
- **Tracking demo** with mock data, a "not found" state and dates relative to today
- Accessibility: labelled form fields, dialog roles, focus trapping and Escape to close in the modal, `aria-expanded` on the menu, reduced-motion support
- Active navigation highlighting while scrolling

## Quick start

1. Open `kimtech-logistics.html` in a browser to preview the design.
2. **Set up your form endpoints** (see the next section). The forms will not send anything until you do.
3. Replace the placeholder content (see the launch checklist).
4. Upload the file to a web host.

## Form endpoints (required)

Both forms ship with a placeholder address:

```
https://formspree.io/f/YOUR_FORM_ID
```

This address does not exist, so submitting either form shows an error ("We could not send your request..." or "We could not send your message..."). This is intended: it is better to show a failure than to tell visitors a request was sent when it was not.

There are **two forms**, each with its own `data-endpoint` attribute. Search the file for `YOUR_FORM_ID` and replace it in both places:

```html
<form id="quote-form"   data-endpoint="https://formspree.io/f/YOUR_FORM_ID" ...>
<form id="contact-form" data-endpoint="https://formspree.io/f/YOUR_FORM_ID" ...>
```

### Option A: Formspree (works with the code as written)

1. Create an account at [formspree.io](https://formspree.io).
2. Click **New Form** and enter the email address that should receive submissions.
3. Copy the form URL, for example `https://formspree.io/f/xyzabcde`.
4. Paste it into the `data-endpoint` of each form.
5. Submit a test message. Formspree may ask you to confirm your email on the first submission.

### Recommendations

- **Use two separate Formspree forms**, for example "KimTech Quotes" and "KimTech Contact". This keeps quote requests apart from general messages and lets you route them to different inboxes or team members.
- **Test on a real host, not by double-clicking the file.** Some form services reject requests from pages opened as `file://`. Test on your live domain, a staging URL, Netlify, GitHub Pages or Vercel.
- **Send a test submission after every change** and check that it reaches the inbox, including the spam folder.
- **Restrict the form to your domain** if your provider offers it (Formspree lets you allow-list domains). This reduces spam and stops others from using your endpoint.
- **Turn on spam protection.** The contact form already includes a hidden `_gotcha` field that Formspree uses as a honeypot. Consider also enabling reCAPTCHA or hCaptcha in your form dashboard.
- **Do not put secret keys in this file.** Anything in the HTML is visible to every visitor. Form IDs and public access keys are fine; private API keys are not.
- **Add a privacy notice.** If you collect names, emails and phone numbers, link a privacy policy (the footer links to `/privacy` and `/terms`, which you need to create) and follow the data protection rules that apply to your visitors.
- **Keep the error message.** If the endpoint goes down, visitors will see an error instead of a false success message. Consider also showing a direct email address as a fallback.

### Option B: Other providers

| Provider | What to change |
|---|---|
| **Web3Forms** | Set `data-endpoint` to `https://api.web3forms.com/submit` and add `<input type="hidden" name="access_key" value="YOUR_ACCESS_KEY">` inside each form. |
| **Netlify Forms** | Needs a code change: add `data-netlify="true"` and a hidden `form-name` field, and post to the page URL instead of an external endpoint. Works only when hosted on Netlify. |
| **EmailJS** | Needs a code change: replace the `fetch` call in the submit handlers with the EmailJS SDK. |
| **Your own API** | Point `data-endpoint` at your server. It must accept a `POST` with form data, return a 2xx status on success, and allow requests from your site (CORS). |

The submit handlers treat any non-2xx response as a failure.

### Troubleshooting form errors

1. Confirm `YOUR_FORM_ID` is replaced in **both** forms.
2. Open the browser developer tools (F12), go to the **Network** tab, submit the form and check the status code:
   - `404`: the form ID is wrong.
   - `403`: the domain is not allowed, or the form is not confirmed or activated.
   - `422`: a required field is missing or invalid.
   - `429`: too many submissions in a short time.
   - Blocked or failed with no status: a network, CORS or `file://` issue. Test on a real host.
3. Check the **Console** tab for JavaScript errors.

## Customizing

### Content

Most repeated content lives in data arrays in the `<script>` at the bottom of the file:

| Array | Controls |
|---|---|
| `trust` | The four trust items in the hero |
| `stats` | The global reach numbers |
| `services` | The six service cards |
| `reviews` | The testimonials |
| `news` | The news cards |
| `shipments` | Demo tracking data |

Edit the arrays instead of the HTML.

### Colors

Colors are defined in `tailwind.config` near the top of the file: `brand` (orange) and `navy`. Change the hex values there and the whole site follows. Dark mode surface colors are in the CSS block marked `/* Dark mode */`.

### Tracking

The tracking tool is a **demo**. It only recognizes the IDs in the `shipments` object (`KT-8942-X` and `KT-1001-A`). To make it real, replace the lookup in `trackShipment()` with a call to your tracking API and render the response in the same layout.

## Launch checklist

- [ ] Replace `YOUR_FORM_ID` in both forms and test each one
- [ ] Replace the placeholder phone, email, address and hours (in the contact section and the footer)
- [ ] Replace the placeholder statistics (200+ countries, 100K+ shipments and so on) with figures you can verify, or remove them
- [ ] Replace the sample testimonials with real, permissioned reviews. Fake testimonials can breach advertising and consumer protection rules.
- [ ] Replace the sample news posts with real articles, or remove the section
- [ ] Add real social media links in the footer
- [ ] Create the `/privacy` and `/terms` pages
- [ ] Review claims such as insurance and delivery performance so they match what you actually offer
- [ ] Add a favicon and an Open Graph image (see the `TODO` comment in the `<head>`)
- [ ] Replace the demo tracking data with a real tracking source, or label it clearly as a demo

## Production recommendations

- **Replace the Tailwind CDN script.** `cdn.tailwindcss.com` is meant for prototyping and is slower. For production, build the CSS with the Tailwind CLI.
- **Host your own images.** The page loads photos from Unsplash by URL. Image IDs can be removed over time, so download the photos you want (check the license) and serve them from your site, in sized and compressed versions.
- **Load Font Awesome selectively**, or switch to inline SVG icons, to avoid downloading the full icon library.
- **Add HTTPS.** Required for secure form submissions. Most hosts provide it for free.

## Browser support

Works in current versions of Chrome, Edge, Firefox and Safari.

## Project notes

- Single file: HTML, CSS and JavaScript live together in `kimtech-logistics.html`.
- Dependencies loaded from CDNs: Tailwind CSS, Google Fonts (Inter), Font Awesome and Unsplash images.
- The theme choice is stored in the browser's `localStorage` under the key `theme`.
