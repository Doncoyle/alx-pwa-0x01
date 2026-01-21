# CineSeek Movie Discovery App

A modern movie discovery application built with Next.js 15, React 19, and TypeScript. CineSeek leverages the MoviesDatabase API to provide users with comprehensive movie information, filtering capabilities, and an intuitive search experience.

## API Overview

The MoviesDatabase API provides programmatic access to a vast catalog of movies and TV shows, including metadata such as titles, genres, release years, ratings, cast information, and poster images. It supports filtering, sorting, and pagination—ideal for building movie discovery applications like CineSeek.

## Version

v1.0 (Stable API, last updated Nov 2024)

## Available Endpoints

### 1. GET `/titles` — Fetch list of movies
- **Parameters:**
  - `limit` (integer, 1–250): Number of results per page (default: 50)
  - `page` (integer): Page number for pagination
  - `year` (integer): Filter by release year (e.g., 2023)
  - `genre` (string): Filter by genre (e.g., "Action", "Drama")
  - `sort` (string): Sort results (e.g., "year.desc" for newest first)

- **Response (200 OK):**
\`\`\`json
{
  "page": 1,
  "next": "/titles?year=2023&page=2",
  "entries": 12,
  "results": [
    {
      "id": "tt15398776",
      "primaryImage": {
        "url": "https://example.com/poster.jpg",
        "width": 1000,
        "height": 1500
      },
      "titleText": {
        "text": "Oppenheimer"
      },
      "releaseYear": {
        "year": 2023
      },
      "runtimeMinutes": 180,
      "genres": ["Biography", "Drama", "History"],
      "imdbRating": 8.1
    }
  ]
}
\`\`\`

### 2. GET `/titles/{id}` — Fetch movie details
### 3. GET `/titles/{id}/images` — Fetch movie images
### 4. GET `/titles/{id}/reviews` — Fetch user reviews

## Authentication

### Required Headers:
\`\`\`
X-RapidAPI-Key: <your_api_key>
X-RapidAPI-Host: moviesdatabase.p.rapidapi.com
\`\`\`

**Important Security Notes:**
- Never expose your API key in client-side code
- Always use a server-side proxy (e.g., `/api/fetch-movies`) to hide the API key
- Store the API key securely in environment variables (`MOVIE_API_KEY`)
- No OAuth or user login required—authentication is API-key based only

## Error Handling Strategy

| Status | Meaning | Handling in Code |
|--------|---------|------------------|
| **401** | Unauthorized | Invalid or missing API key. Show "API configuration error" in development; log securely. |
| **403** | Forbidden | Plan limit exceeded. Show "Rate limit reached" UI; disable search/filter buttons. |
| **429** | Too Many Requests | Rate-limited by RapidAPI. Implement exponential backoff retry logic. |
| **404** | Not Found | Invalid ID or endpoint. Fallback to default year/genre; show user-friendly error message. |
| **5xx** | Server Error | Temporary server issue. Retry up to 2 times; else show toast notification to user. |

## Usage Limits & Best Practices

### Rate Limits:
- **Free tier:** ~100 requests/day (RapidAPI default)
- **Pro tier:** Higher limits available (check RapidAPI dashboard)

### Optimization Recommendations:
1. **Pagination:** Always paginate with `limit=12, page=n` to avoid fetching excessive data
2. **Client-side Caching:** Use `useMemo`, SWR, or React Query to cache API responses
3. **Server-side Proxy:** Route all API calls through `/api/fetch-movies` to hide credentials
4. **Input Validation:** Pre-validate user inputs (e.g., year between 1900–2025)
5. **Fallback Images:** Provide a default placeholder if `primaryImage.url` is null
6. **Environment Variables:** Use `.env.local` for local development; configure in Vercel dashboard for production

## Project Structure

\`\`\`
├── app/
│   ├── layout.tsx              # Root layout with metadata
│   ├── page.tsx                # Landing page with hero section
│   ├── movies/
│   │   └── page.tsx            # Movies discovery page with filters
│   ├── api/
│   │   └── fetch-movies/
│   │       └── route.ts        # Secure API proxy for MoviesDatabase
│   └── globals.css             # Global styles
├── components/
│   ├── commons/
│   │   ├── button.tsx          # Reusable button component
│   │   ├── loading.tsx         # Loading spinner component
│   │   └── movie-card.tsx      # Movie card display component
│   └── layouts/
│       ├── header.tsx          # App header
│       ├── footer.tsx          # App footer
│       └── layout.tsx          # Main layout wrapper
└── interfaces/
    └── index.ts                # TypeScript interfaces for API responses
\`\`\`

## Setup Instructions

1. **Clone the repository:**
   \`\`\`bash
   git clone https://github.com/your-username/cineseek-movie-app.git
   cd cineseek-movie-app
   \`\`\`

2. **Install dependencies:**
   \`\`\`bash
   npm install
   \`\`\`

3. **Get your API key:**
   - Visit [MoviesDatabase on RapidAPI](https://rapidapi.com/hackathondevs/api/moviesdatabase/)
   - Sign up or log in
   - Subscribe to the free tier
   - Copy your API key from the dashboard

4. **Configure environment variables:**
   - Create a `.env.local` file in the root directory
   - Add your API key:
     \`\`\`
     MOVIE_API_KEY=<your_rapidapi_key>
     \`\`\`

5. **Run the development server:**
   \`\`\`bash
   npm run dev
   \`\`\`
   Open [http://localhost:3000](http://localhost:3000) in your browser.

6. **Deploy to Vercel:**
   \`\`\`bash
   npm run build
   vercel deploy
   \`\`\`
   Then add your `MOVIE_API_KEY` environment variable in the Vercel dashboard.

## Technology Stack

- **Frontend:** Next.js 15, React 19, TypeScript
- **Styling:** Tailwind CSS v4
- **API:** MoviesDatabase (via RapidAPI)
- **Deployment:** Vercel

## Features

- Landing page with hero section and call-to-action
- Movie discovery page with search and filtering
- Filter by year and genre
- Responsive design for mobile and desktop
- Loading states and error handling
- Server-side API proxy for security
- Full TypeScript support

## License

MIT

## Contributing

Contributions are welcome! Please open an issue or submit a pull request with your improvements.

---

**Deployed on Vercel:** [https://vercel.com/doncoyles-projects/v0-cine-seek-movie-app](https://vercel.com/doncoyles-projects/v0-cine-seek-movie-app)

**Built with v0.app:** [https://v0.app/chat/nHV2BW4kgx0](https://v0.app/chat/nHV2BW4kgx0)
