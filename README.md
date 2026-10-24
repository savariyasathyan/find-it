# FindIt — Campus Lost and Found Portal

> **A modern, full-stack MERN capstone portal designed for college campuses to connect students who lost belongings with finders quickly, securely, and transparently.**

---

## 📌 Project Overview

**FindIt** is a full-stack web platform built for college students and staff. Instead of fragmented campus group chats or crowded cork bulletin boards, FindIt provides a centralized, searchable, real-time database of lost and found items across campus buildings.

### Key Capabilities
- 🔍 **Real-Time Search & Multi-Filter Directory**: Search items by keyword, filter by Type (Lost/Found), Campus Building/Zone, Category, and Resolution Status.
- ⚡ **AI Smart-Match Recommendations**: Automatically compares lost item submissions against found reports in the same location and category, surfacing potential matches instantly.
- 🛡️ **Anti-Fraud Claim Verification**: Finders can pose secret verification questions (e.g. *"What wallpaper is on the phone?"* or *"What sticker is on the laptop?"*) to ensure only the true owner claims the item.
- 🤝 **Safe Campus Handoff & Reunion Tracking**: Seamlessly coordinates returns through Campus Security desks and tracks portal recovery rates.
- 📱 **Responsive Campus-Themed UI**: Built with Tailwind CSS in white, navy blue, and teal accents.

---

## 🛠️ Technology Stack

| Layer | Technology | Description |
|---|---|---|
| **Frontend** | React 18, Vite | High-performance SPA with modern React hooks |
| **Styling** | Tailwind CSS | Clean college-campus theme (Navy Blue `#0f1f38`, Teal `#0d9488`, White) |
| **Icons** | Lucide React | Lightweight, accessible iconography |
| **Routing** | React Router v6 | Client-side routing with URL query state persistence |
| **Backend** | Node.js, Express.js | Scalable RESTful API with validation and centralized error handling |
| **Database** | MongoDB & Mongoose | Document database with text indexes and schemas |
| **File Uploads**| Multer | Secure multipart image upload with category fallback thumbnails |
| **Environment**| dotenv | Safe configuration for ports, database URIs, and CORS |

---

## 📁 Project Architecture

```
find it/
├── backend/
│   ├── src/
│   │   ├── config/
│   │   │   └── db.js            # MongoDB Atlas connection & in-memory dev fallback
│   │   ├── controllers/
│   │   │   ├── itemController.js   # CRUD, filters, search, smart-match, user query
│   │   │   ├── claimController.js  # Claim submission and verification workflow
│   │   │   └── statsController.js  # Campus live statistics & metrics
│   │   ├── middleware/
│   │   │   ├── errorHandler.js     # Centralized error handler & status codes
│   │   │   └── upload.js           # Multer configuration for file uploads
│   │   ├── models/
│   │   │   ├── Item.js             # Mongoose Schema with text indexes
│   │   │   └── Claim.js            # Mongoose Schema for claim submissions
│   │   ├── routes/
│   │   │   ├── itemRoutes.js       # /api/items endpoints
│   │   │   ├── claimRoutes.js      # /api/claims endpoints
│   │   │   └── statsRoutes.js      # /api/stats endpoints
│   │   ├── utils/
│   │   │   └── seedData.js         # Realistic campus seed dataset
│   │   └── server.js               # Express application entrypoint
│   ├── uploads/                    # Directory for uploaded item images
│   ├── .env.example
│   ├── .env
│   └── package.json
├── frontend/
│   ├── src/
│   │   ├── api/
│   │   │   └── itemApi.js          # Axios API client
│   │   ├── components/
│   │   │   ├── Navbar.jsx          # Campus header with live quick search
│   │   │   ├── Footer.jsx          # Security contacts and campus links
│   │   │   ├── ItemCard.jsx        # Responsive card with status badges
│   │   │   ├── ItemFilter.jsx      # Multi-criteria filter & sort bar
│   │   │   ├── ClaimModal.jsx      # Claim submission modal with proof verification
│   │   │   ├── ResolveModal.jsx    # Reporter reunite modal with passcode
│   │   │   ├── SmartMatchBanner.jsx# Automated counter-item matching banner
│   │   │   ├── StatsBanner.jsx     # Campus recovery statistics counter
│   │   │   └── LoadingSpinner.jsx  # Spinners and skeleton placeholders
│   │   ├── pages/
│   │   │   ├── HomePage.jsx        # Landing hero, stats, categories, recent items
│   │   │   ├── BrowsePage.jsx      # Searchable directory with list/grid toggle
│   │   │   ├── ItemDetailPage.jsx  # Detailed item view, contact, claim, resolve
│   │   │   ├── ReportItemPage.jsx  # Lost / Found submission form with image upload
│   │   │   ├── MyReportsPage.jsx   # Track and manage user's reported items
│   │   │   ├── HowItWorksPage.jsx  # Student guidelines, security desk, and FAQs
│   │   │   └── NotFoundPage.jsx    # 404 page
│   │   ├── context/
│   │   │   └── ToastContext.jsx    # Custom toast notifications
│   │   ├── App.jsx                 # Routes configuration
│   │   ├── main.jsx                # React root
│   │   └── index.css               # Tailwind directives and custom scrollbar
│   ├── index.html
│   ├── vite.config.js              # Proxy setup for /api and /uploads
│   ├── tailwind.config.js
│   └── package.json
└── README.md
```

---

## 🚀 Getting Started

### 1. Prerequisites
- **Node.js** (v18 or higher recommended)
- **npm** (v9 or higher)
- **MongoDB**: Local MongoDB instance or free [MongoDB Atlas](https://www.mongodb.com/cloud/atlas) cluster.

---

### 2. Backend Setup

```bash
# Navigate to the backend directory
cd backend

# Install dependencies
npm install

# Configure environment variables (a default .env is included)
# If using MongoDB Atlas, update MONGODB_URI in backend/.env:
# MONGODB_URI=mongodb+srv://<user>:<password>@cluster0.mongodb.net/findit?retryWrites=true&w=majority

# Populate the database with realistic campus test data
npm run seed

# Start the backend server in development mode
npm run dev
```

The backend server runs at `http://localhost:5000`.

---

### 3. Frontend Setup

In a new terminal window:

```bash
# Navigate to the frontend directory
cd frontend

# Install dependencies
npm install

# Start the Vite development server
npm run dev
```

Open your browser at `http://localhost:5173`.

---

## 📡 REST API Reference

### Items (`/api/items`)
- `GET /api/items`: Retrieve items with filtering (`type`, `category`, `building`, `status`, `search`, `sortBy`, `page`, `limit`).
- `GET /api/items/recent`: Retrieve the most recent active items for homepage.
- `GET /api/items/:id`: Retrieve single item details and increment view count.
- `POST /api/items`: Create a new lost or found report (supports multipart image file or `imageUrl`).
- `PUT /api/items/:id`: Update item details (requires reporter passcode or email).
- `PATCH /api/items/:id/resolve`: Mark item as resolved/reunited with resolution notes.
- `DELETE /api/items/:id`: Remove item (requires passcode).
- `GET /api/items/:id/matches`: Retrieve AI Smart-Match counter items.
- `GET /api/items/user/:email`: Retrieve reports submitted by a specific student email.

### Claims (`/api/claims`)
- `POST /api/claims`: Submit proof of ownership to claim an item.
- `GET /api/claims/item/:itemId`: Retrieve all submitted claims for an item.
- `PATCH /api/claims/:id/status`: Approve or reject a claim.

### Statistics (`/api/stats`)
- `GET /api/stats`: Total items, active lost, active found, recovery rate, category distribution, top campus locations.

---

## 🎓 Capstone Presentation Highlights

1. **Clean Code & Separation of Concerns**: Modular controller-service-route-model architecture on backend; reusable component hierarchy on frontend.
2. **Resilient Database Configuration**: Automatically connects to MongoDB Atlas or local MongoDB with zero crash risk.
3. **Smart Matching Algorithm**: Keyword and category matching algorithm to reduce duplicate reports.
4. **Security & Privacy First**: Private verification questions protect valuable electronics from fraudulent claims.
5. **Campus Ready**: Built with realistic college categories, campus buildings, and honor code workflows.
