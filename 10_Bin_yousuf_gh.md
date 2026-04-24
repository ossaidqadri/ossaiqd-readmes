<img src="https://socialify.git.ci/ossaidqadri/Bin_yousuf/image?custom_description=&description=1&font=KoHo&forks=1&language=1&name=1&owner=1&pattern=Charlie+Brown&stargazers=1&theme=Auto" alt="Bin_yousuf" width="640" height="320" />

[![Astro](https://img.shields.io/badge/Astro-5.10.0-orange?style=flat-square&logo=astro)](https://astro.build/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.6.3-blue?style=flat-square&logo=typescript)](https://www.typescriptlang.org/)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-3.4.15-38B2AC?style=flat-square&logo=tailwind-css)](https://tailwindcss.com/)

A modern responsive website for Bin Yousuf Group, a premium real estate development company in Karachi, Pakistan. Built with Astro, React, TypeScript, and GSAP animations.

## Features

- Responsive property showcases with interactive galleries
- GSAP-powered smooth animations and transitions
- Google Sheets API integration for reliable form submissions
- SEO optimization with structured data
- Server-side rendering with Astro
- Real-time lead management system
- WhatsApp integration for direct contact

## Tech Stack

- **Frontend**: Astro 5.10.0, React 18.3.1, TypeScript 5.6.3
- **Styling**: Tailwind CSS 3.4.15, GSAP animations
- **APIs**: Google Sheets API (googleapis)
- **Deployment**: Vercel
- **Development**: Bun, Vite, PostCSS

## Getting Started

### Prerequisites

- Node.js 18.0 or higher
- Bun (recommended) or npm/yarn

### Installation

1. Clone the repository

   ```bash
   git clone https://github.com/imossaidqadri/Bin_yousuf.git
   cd Bin_yousuf
   ```

2. Install dependencies

   ```bash
   bun install
   ```

3. Set up environment variables

   ```bash
   cp .env.example .env
   ```

4. Run the development server

   ```bash
   bun dev
   ```

5. Open [http://localhost:4321](http://localhost:4321)

### Build for Production

```bash
bun run build    # Build application
bun run preview  # Preview production build
```

## Available Scripts

| Command       | Description              |
| ------------- | ------------------------ |
| `bun dev`     | Start development server |
| `bun build`   | Build for production     |
| `bun preview` | Preview production build |
| `bun astro`   | Run Astro CLI commands   |

## Configuration

### Environment Variables

Create a `.env` file:

```env
NEXT_PUBLIC_SITE_URL=https://www.binyousufgroup.com
CONTACT_NUMBER=+923360878079
GOOGLE_SHEET_ID=your_sheet_id
GOOGLE_PROJECT_ID=your_project_id
GOOGLE_PRIVATE_KEY_ID=your_private_key_id
GOOGLE_PRIVATE_KEY="-----BEGIN PRIVATE KEY-----\nyour_private_key\n-----END PRIVATE KEY-----\n"
GOOGLE_CLIENT_EMAIL=your_service_account@your_project.iam.gserviceaccount.com
GOOGLE_CLIENT_ID=your_client_id
GOOGLE_CLIENT_X509_CERT_URL=your_cert_url
```

## Project Structure

```
src/
├── components/          # React components
├── pages/              # Astro pages and API routes
├── layouts/            # Astro layouts
├── data/               # Property data and configuration
├── hooks/              # Custom React hooks
├── types/              # TypeScript types
└── assets/             # Images and media

public/
├── images/             # Property images and media
└── videos/             # Property showcase videos
```

### Key Features

- Google Sheets API integration with service account authentication
- All form fields optional for better conversion rates
- Real-time lead management and tracking
- WhatsApp integration for direct contact
- SEO optimization with structured data markup

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/name`)
3. Commit changes (`git commit -m 'Add feature'`)
4. Push to branch (`git push origin feature/name`)
5. Open a Pull Request

## License

This project is proprietary and confidential. All rights reserved by Bin Yousuf Group.
Website development by The Other Dev.

## Support

For technical support:

- **Email**: hello@otherdev.com
- **Website**: [The Other Dev](https://otherdev.com)
- **LinkedIn**: [The Other Dev](https://www.linkedin.com/company/theotherdev)

---

© 2025 Bin Yousuf Group. All rights reserved. Website by The Other Dev.
