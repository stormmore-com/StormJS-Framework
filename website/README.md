# BoltJS Framework - Website

Nowoczesna, minimalistyczna strona internetowa dla projektu BoltJS Framework.

## 📁 Struktura

```
website/
├── index.html          # Główna strona
├── css/
│   └── style.css      # Style CSS
├── js/
│   └── main.js        # JavaScript (interaktywność)
├── assets/            # Zasoby (obrazy, ikony)
└── README.md          # Ten plik
```

## 🚀 Hosting

### Opcja 1: Własny Serwer (Apache/Nginx)

#### Apache

1. Skopiuj zawartość katalogu `website/` do katalogu serwera:
   ```bash
   cp -r website/* /var/www/html/boltjs/
   ```

2. Skonfiguruj VirtualHost w Apache:
   ```apache
   <VirtualHost *:80>
       ServerName boltjs.example.com
       DocumentRoot /var/www/html/boltjs

       <Directory /var/www/html/boltjs>
           Options -Indexes +FollowSymLinks
           AllowOverride All
           Require all granted
       </Directory>
   </VirtualHost>
   ```

3. Włącz konfigurację i zrestartuj Apache:
   ```bash
   sudo a2ensite boltjs
   sudo systemctl restart apache2
   ```

#### Nginx

1. Skopiuj pliki:
   ```bash
   cp -r website/* /var/www/boltjs/
   ```

2. Skonfiguruj Nginx:
   ```nginx
   server {
       listen 80;
       server_name boltjs.example.com;
       root /var/www/boltjs;
       index index.html;

       location / {
           try_files $uri $uri/ =404;
       }

       # Gzip compression
       gzip on;
       gzip_types text/css application/javascript text/html;
   }
   ```

3. Testuj i zrestartuj Nginx:
   ```bash
   sudo nginx -t
   sudo systemctl restart nginx
   ```

### Opcja 2: Docker

Stwórz `Dockerfile`:

```dockerfile
FROM nginx:alpine
COPY website/ /usr/share/nginx/html/
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

Zbuduj i uruchom:
```bash
docker build -t boltjs-website .
docker run -d -p 80:80 boltjs-website
```

### Opcja 3: Node.js (Express)

Stwórz prosty serwer:

```javascript
// server.js
const express = require('express');
const path = require('path');
const app = express();

app.use(express.static('website'));

app.get('*', (req, res) => {
    res.sendFile(path.join(__dirname, 'website', 'index.html'));
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
    console.log(`Server running on port ${PORT}`);
});
```

Uruchom:
```bash
npm install express
node server.js
```

### Opcja 4: Statyczne Platformy Hostingowe (Alternatywy)

Jeśli chcesz łatwiejszą opcję bez własnego serwera:

#### Netlify
```bash
# Install Netlify CLI
npm install -g netlify-cli

# Deploy
cd website
netlify deploy --prod
```

#### Vercel
```bash
# Install Vercel CLI
npm install -g vercel

# Deploy
cd website
vercel --prod
```

#### GitHub Pages
```bash
# Create gh-pages branch
git checkout -b gh-pages
git add website/*
git commit -m "Deploy website"
git subtree push --prefix website origin gh-pages
```

## 🔒 SSL/HTTPS

### Certbot (Let's Encrypt)

Dla Apache:
```bash
sudo apt install certbot python3-certbot-apache
sudo certbot --apache -d boltjs.example.com
```

Dla Nginx:
```bash
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d boltjs.example.com
```

### Cloudflare

1. Dodaj domenę do Cloudflare
2. Zmień nameservery domeny
3. Włącz SSL w ustawieniach Cloudflare (Full lub Full Strict)

## ⚙️ Konfiguracja Domeny

### DNS Records

Dodaj odpowiednie rekordy DNS:

```
Type    Name    Value               TTL
A       @       your.server.ip      300
A       www     your.server.ip      300
```

Lub dla Cloudflare/CDN:
```
Type    Name    Value               Proxy Status
A       @       your.server.ip      Proxied
CNAME   www     boltjs.example.com  Proxied
```

## 🎨 Dostosowanie

### Zmiana Kolorów

Edytuj zmienne CSS w `css/style.css`:

```css
:root {
    --primary-color: #3b82f6;      /* Główny kolor */
    --secondary-color: #8b5cf6;    /* Kolor drugorzędny */
    --text-primary: #1e293b;       /* Kolor tekstu */
    --text-secondary: #64748b;     /* Kolor tekstu drugorzędnego */
}
```

### Dodawanie Sekcji

1. Dodaj HTML w `index.html`
2. Dodaj style w `css/style.css`
3. Dodaj interaktywność w `js/main.js` (opcjonalnie)

### Zmiana Treści

Wszystkie teksty są w `index.html`. Sekcje:
- Hero (linie 31-64)
- Features (linie 67-108)
- Quick Start (linie 111-157)
- Examples (linie 160-196)
- CTA (linie 199-213)
- Footer (linie 216-244)

## 📊 Analytics (Opcjonalnie)

### Google Analytics

Dodaj przed `</head>` w `index.html`:

```html
<!-- Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=GA_MEASUREMENT_ID"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'GA_MEASUREMENT_ID');
</script>
```

### Plausible Analytics (Privacy-friendly)

```html
<script defer data-domain="yourdomain.com" src="https://plausible.io/js/script.js"></script>
```

## 🔧 Optymalizacja

### Minifikacja

Zminifikuj CSS i JS dla produkcji:

```bash
# CSS
npx cssnano css/style.css css/style.min.css

# JavaScript
npx terser js/main.js -o js/main.min.js
```

Następnie zaktualizuj linki w `index.html`.

### Caching

Dodaj nagłówki cache w konfiguracji serwera:

**Nginx:**
```nginx
location ~* \.(css|js|jpg|jpeg|png|gif|ico|svg)$ {
    expires 1y;
    add_header Cache-Control "public, immutable";
}
```

**Apache (.htaccess):**
```apache
<IfModule mod_expires.c>
    ExpiresActive On
    ExpiresByType text/css "access plus 1 year"
    ExpiresByType application/javascript "access plus 1 year"
    ExpiresByType image/jpeg "access plus 1 year"
    ExpiresByType image/png "access plus 1 year"
</IfModule>
```

## 📱 Testowanie

### Responsywność
- Otwórz DevTools (F12)
- Przełącz na widok mobile (Ctrl+Shift+M)
- Testuj na różnych rozmiarach ekranu

### Performance
- [Google PageSpeed Insights](https://pagespeed.web.dev/)
- [GTmetrix](https://gtmetrix.com/)
- [WebPageTest](https://www.webpagetest.org/)

### Kompatybilność
- Testuj w różnych przeglądarkach (Chrome, Firefox, Safari, Edge)
- Sprawdź na urządzeniach mobilnych

## 🐛 Troubleshooting

### Problem: Czcionki nie ładują się
**Rozwiązanie:** Sprawdź połączenie z Google Fonts lub dodaj fallback fonty.

### Problem: Style CSS nie działają
**Rozwiązanie:** Sprawdź ścieżkę do `css/style.css` i uprawnienia plików.

### Problem: JavaScript nie działa
**Rozwiązanie:** Otwórz konsolę przeglądarki (F12) i sprawdź błędy.

### Problem: 404 na serwerze
**Rozwiązanie:** Sprawdź DocumentRoot i uprawnienia katalogów (755 dla katalogów, 644 dla plików).

## 📝 Licencja

Strona internetowa jest częścią projektu BoltJS Framework i podlega tej samej licencji MIT.

## 🤝 Wsparcie

- 📖 [Dokumentacja BoltJS](https://stormmoredev.github.io/bolt-js-framework/)
- 💬 [GitHub Issues](https://github.com/stormmoredev/bolt-js-framework/issues)
- ⭐ [GitHub Repository](https://github.com/stormmoredev/bolt-js-framework)

---

Wykonane z ⚡ przez społeczność BoltJS
