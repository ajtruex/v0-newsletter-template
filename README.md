# v0 Newsletter Template

A modern, production-ready email capture app designed to help you grow your audience with confidence. Built with Next.js 15, TypeScript, and Radix UI components.

## ✨ Features

### Core Functionality
- 📧 **Email Capture** - Clean, intuitive email subscription form
- ✅ **Built-in Validation** - Real-time email validation with error handling
- 🎨 **Smooth Animations** - Polished UI interactions powered by Framer Motion
- 💾 **Upstash Redis** - Reliable, serverless data storage for subscriber emails
- 🚀 **Production Ready** - Optimized and ready to deploy

### Tech Highlights
- ⚡ **Next.js 15** - Latest version with React 19 for optimal performance
- 🎨 **Tailwind CSS** - Beautiful, responsive design out of the box
- 🧩 **Radix UI** - Accessible, customizable component primitives
- 📝 **TypeScript** - Type-safe development experience
- 🌓 **Dark Mode** - Built-in theme switching
- 📊 **Analytics Ready** - Vercel Analytics integration
- 🎯 **Form Validation** - React Hook Form with Zod schemas

## 🚀 Quick Start

### Prerequisites

- Node.js 18+ 
- npm (recommended), npm, or yarn
- Upstash Redis account (free tier available)

### Installation

1. **Clone the repository:**
```bash
git clone https://github.com/ajtruex/v0-newsletter-template.git
cd v0-newsletter-template
```

2. **Install dependencies:**
```bash
npm install
```

3. **Set up environment variables:**

Create a `.env.local` file in the root directory:

```env
UPSTASH_REDIS_REST_URL=your_redis_url
UPSTASH_REDIS_REST_TOKEN=your_redis_token
```

Get your credentials from [Upstash Console](https://console.upstash.com/)

4. **Run the development server:**
```bash
npm run dev
```

5. **Open your browser:**

Navigate to [http://localhost:3000](http://localhost:3000) to see your email capture form in action!

## 📋 Available Scripts

| Command | Description |
|---------|-------------|
| `npm run dev` | Start development server at localhost:3000 |
| `npm run build` | Create optimized production build |
| `npm run start` | Run production server |
| `npm run lint` | Check code quality with ESLint |

## 📁 Project Structure

```
v0-newsletter-template/
├── app/                 # Next.js app directory (routes & pages)
├── components/          # Reusable React components
│   ├── ui/             # Base UI components (buttons, forms, etc.)
│   └── ...             # Feature components
├── hooks/              # Custom React hooks
├── lib/                # Utility functions and helpers
│   └── utils.ts        # Common utilities
├── public/             # Static assets (images, fonts, etc.)
├── styles/             # Global CSS and Tailwind config
└── package.json        # Project dependencies and scripts
```

## 🎯 Use Cases

Perfect for:
- 📱 **Landing Pages** - Drop into your product launch page
- 🎉 **Marketing Sites** - Collect leads from campaigns
- 📰 **Newsletter Signups** - Build your email list
- 🚀 **Waitlists** - Manage early access signups
- 💼 **Lead Generation** - Capture potential customers

## 🎨 Customization

### Styling

Customize the theme in `tailwind.config.js`:

```js
module.exports = {
  theme: {
    extend: {
      colors: {
        // Add your brand colors
      },
    },
  },
}
```

### Email Validation

Validation rules can be customized using Zod schemas in your form components.

### Components

All UI components are in the `components/` directory and can be easily adapted to match your brand identity.

## 🔧 Configuration

### Upstash Redis Setup

1. Create a free account at [Upstash](https://upstash.com/)
2. Create a new Redis database
3. Copy the REST API credentials
4. Add them to your `.env.local` file

### Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `UPSTASH_REDIS_REST_URL` | Your Upstash Redis REST URL | Yes |
| `UPSTASH_REDIS_REST_TOKEN` | Your Upstash Redis REST token | Yes |

## 🚢 Deployment

### Deploy to Vercel (Recommended)

The fastest way to deploy:

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https://github.com/ajtruex/v0-newsletter-template)

**Don't forget to add your environment variables in the Vercel dashboard!**

### Other Platforms

This app works on any platform supporting Node.js:

- **Netlify** - Add build command: `npm build`
- **Railway** - Automatic deployment from GitHub
- **Render** - One-click deployment
- **Cloudflare Pages** - Edge deployment
- **AWS Amplify** - Scalable hosting

## 📦 Built With

- [Next.js](https://nextjs.org/) - React framework
- [TypeScript](https://www.typescriptlang.org/) - Type safety
- [Tailwind CSS](https://tailwindcss.com/) - Styling
- [Radix UI](https://www.radix-ui.com/) - UI primitives
- [Framer Motion](https://www.framer.com/motion/) - Animations
- [Upstash Redis](https://upstash.com/) - Serverless database
- [React Hook Form](https://react-hook-form.com/) - Form management
- [Zod](https://zod.dev/) - Schema validation
- [Lucide Icons](https://lucide.dev/) - Icon library

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

## 💬 Support

- 🐛 **Found a bug?** [Open an issue](https://github.com/ajtruex/v0-newsletter-template/issues)
- 💡 **Have a feature request?** [Start a discussion](https://github.com/ajtruex/v0-newsletter-template/discussions)
- ⭐ **Like this project?** Give it a star!

## 🙏 Acknowledgments

Built with ❤️ using [v0](https://v0.dev) by Vercel

---

**Ready to grow your audience?** Drop this into your landing page and start collecting emails without the hassle! 🚀
