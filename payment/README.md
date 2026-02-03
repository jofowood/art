# Payment QR Codes Page

A simple, responsive HTML page displaying payment QR codes for PayPal, Venmo, and Zelle.

## Files Included

- `index.html` - Main HTML page
- `paypal-qr.png` - PayPal QR code image
- `venmo-qr.png` - Venmo QR code image
- `zelle-qr.jpg` - Zelle QR code image

## Deploying to GitHub Pages

### Option 1: Quick Deploy (Recommended)

1. Create a new repository on GitHub (e.g., `payment-qr`)
2. Upload all four files to the repository
3. Go to repository Settings → Pages
4. Under "Source", select "Deploy from a branch"
5. Select branch: `main` and folder: `/ (root)`
6. Click Save
7. Your page will be live at: `https://yourusername.github.io/payment-qr/`

### Option 2: Using Git Command Line

```bash
# Create a new directory and initialize git
mkdir payment-qr
cd payment-qr
git init

# Copy your files into this directory
# (copy index.html and all three QR code images here)

# Add and commit files
git add .
git commit -m "Initial commit: Payment QR codes page"

# Create repository on GitHub first, then:
git remote add origin https://github.com/yourusername/payment-qr.git
git branch -M main
git push -u origin main

# Enable GitHub Pages in repository settings
```

## Features

- **Responsive Design**: Works perfectly on mobile and desktop
- **Modern UI**: Clean, professional gradient background
- **Hover Effects**: Cards lift slightly on hover for better interactivity
- **Mobile-First**: Optimized for phone screens since people will scan with their phones
- **Fast Loading**: Static HTML with no external dependencies

## Customization

To customize the page, edit `index.html`:

- Change the title/heading by modifying the `<h1>` tag
- Update the subtitle in the `<p class="subtitle">` tag
- Modify colors in the CSS `background` gradient
- Adjust spacing by changing padding values in the `.payment-card` class

## License

Free to use and modify as needed.
