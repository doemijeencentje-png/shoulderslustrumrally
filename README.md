# Shoulder Lustrum Rally - Website & Registratie

Website voor de IXe Shoulder Lustrum Rally met aanmeldformulier en n8n workflow.

## Bestanden

```
shoulderslustrumrally/
├── index.html                          # Website met registratieformulier
├── images/
│   └── rally-logo.jpg                  # Rally logo
├── n8n-workflow-rally-registration.json # n8n workflow
└── README.md                           # Deze documentatie
```

## Setup Instructies

### 1. Google Sheet Aanmaken

Maak een nieuwe Google Sheet met de volgende kolommen (rij 1):

| A | B | C | D | E | F | G | H | I |
|---|---|---|---|---|---|---|---|---|
| Timestamp | Bestuurder Voornaam | Bestuurder Achternaam | Bestuurder Email | Bestuurder Telefoon | Navigator Voornaam | Navigator Achternaam | Navigator Email | Navigator Telefoon |

**Sheet ID kopiëren:** De URL is `https://docs.google.com/spreadsheets/d/SHEET_ID/edit` - kopieer het `SHEET_ID` deel.

### 2. n8n Workflow Configureren

1. Open n8n (http://localhost:5678)
2. Ga naar **Workflows** → **Import from File**
3. Upload `n8n-workflow-rally-registration.json`
4. Configureer credentials:

   **Google Sheets:**
   - Ga naar de "Google Sheets - Opslaan" node
   - Klik op **Credential** → **Create New**
   - Log in met Google account dat toegang heeft tot de Sheet
   - Vul je Sheet ID in

   **Gmail (shoulderluco2025@gmail.com):**
   - Ga naar de "Email - Bevestiging" node
   - Klik op **Credential** → **Create New**
   - Log in met `shoulderluco2025@gmail.com`
   - Geef n8n toestemming om emails te versturen

5. **Activeer** de workflow (toggle rechtsboven)
6. **Kopieer webhook URL** (klik op Webhook node → Production URL)

### 3. Website Updaten

Open `index.html` en vervang de webhook URL:

```html
<form id="rallyForm" action="https://your-n8n-url/webhook/rally-registration" method="POST">
```

Vervang `https://your-n8n-url/webhook/rally-registration` met je n8n webhook URL.

### 4. Website Hosten

**Optie A - Vercel (gratis):**
```bash
npm i -g vercel
cd shoulderslustrumrally
vercel
```

**Optie B - Lokaal testen:**
```bash
cd shoulderslustrumrally
npx serve .
```

**Optie C - Traditionele hosting:**
Upload de bestanden via FTP naar je webhost.

---

## WhatsApp Groep Aanmaken (na deadline)

Na de inschrijfdeadline kun je een WhatsApp groep maken met alle deelnemers:

### Stap 1: Exporteer telefoonnummers uit Google Sheet

1. Open de Google Sheet
2. Selecteer kolommen E (Bestuurder Telefoon) en I (Navigator Telefoon)
3. Kopieer alle nummers

### Stap 2: Maak WhatsApp groep

**Handmatig:**
1. Open WhatsApp
2. Nieuwe groep → Voeg contacten toe
3. Groepsnaam: "IXe Shoulder Lustrum Rally"

**Via WhatsApp Web (sneller):**
1. Zorg dat alle nummers in je contacten staan
2. Maak groep via WhatsApp Web

### Automatische WhatsApp integratie (optioneel)

Je kunt de n8n workflow uitbreiden met Twilio of WhatsApp Business API:

```javascript
// In n8n Code node - genereer wa.me links
const phones = $json.all_phones; // array van telefoonnummers
const groupInviteMessage = encodeURIComponent(
  "Je bent uitgenodigd voor de WhatsApp groep van de IXe Shoulder Lustrum Rally!"
);

// Genereer individuele invite links
const links = phones.map(phone => {
  const cleanPhone = phone.replace(/[^0-9]/g, '');
  return `https://wa.me/${cleanPhone}?text=${groupInviteMessage}`;
});

return { links };
```

---

## Domein Configuratie

Om de website live te zetten op `shoulderslustrumrally.nl`:

1. **Koop domein** bij bijv. TransIP, Hostnet, of Versio
2. **DNS instellen:**
   - Als je Vercel gebruikt: Voeg domein toe in Vercel dashboard
   - Anders: Wijs A-record naar je server IP

3. **SSL certificaat** wordt automatisch gegenereerd bij Vercel

---

## Testen

1. Open `index.html` in browser (of via `npx serve .`)
2. Vul testgegevens in
3. Controleer:
   - [ ] Bevestigingsmail ontvangen?
   - [ ] Gegevens in Google Sheet?
   - [ ] Success message op website?

---

## Contact

Bij vragen: shoulderluco2025@gmail.com
