# 🗺️ AR Navigation & Shop Discovery App

> **Real-world augmented reality navigation app that overlays nearby shops, ratings, and directions directly on your camera view**

[![MIT License](https://img.shields.io/badge/License-MIT-green.svg)](https://choosealicense.com/licenses/mit/)
[![Android](https://img.shields.io/badge/Platform-Android-brightgreen.svg)](https://developer.android.com/)
[![ARCore](https://img.shields.io/badge/AR-ARCore-blue.svg)](https://developers.google.com/ar)
[![OpenCV](https://img.shields.io/badge/CV-OpenCV-red.svg)](https://opencv.org/)

---

## 📱 What Is This?

An Android app that combines **Google Maps + Yelp + Pokémon GO** vibes. Point your camera at the street, and see:

- 🏪 **Nearby shops floating in AR** with names, ratings, and distance
- ⭐ **Real-time ratings & reviews** from your database
- ➡️ **AR navigation arrows** showing which way to turn
- 🎯 **Smart filters** (open now, rating > 4, category)
- 🔍 **Computer vision enhancements** using OpenCV for better scene understanding

**Example Experience:**
```
You're standing at a road junction 📸
You open the app → camera activates
On screen you see:
🏪 "Sharma Medical – ⭐4.5 (120m)"
🍔 "Burger Point – ⭐4.2 (Turn Right →)"
🟢 Floating arrows on the road showing exact direction
```

---

## 🎯 Core Features

### 🚀 MVP (Version 1.0) - Buildable in 3-4 Weeks

| Feature | Description | Priority |
|---------|-------------|----------|
| 📍 **Location Detection** | GPS + Compass for accurate positioning | ⭐⭐⭐ Must Have |
| 📸 **AR Camera View** | Live camera feed with AR overlays | ⭐⭐⭐ Must Have |
| 🏪 **Shop Pins** | Floating labels showing nearby shops | ⭐⭐⭐ Must Have |
| ⭐ **Basic Info Display** | Name, rating (1-5), distance in meters | ⭐⭐⭐ Must Have |
| 🗺️ **Places API Integration** | Google Places / OpenStreetMap data | ⭐⭐⭐ Must Have |
| 🎨 **Simple UI** | Clean, intuitive interface | ⭐⭐ Should Have |

**MVP Goal:** User can open camera, see nearby shops in AR with ratings and distance.

### 🔥 Version 2.0 - Advanced Features

| Feature | Description | Priority |
|---------|-------------|----------|
| 🧭 **AR Navigation** | Turn-by-turn directions with AR arrows | ⭐⭐⭐ High |
| 🎨 **OpenCV Scene Analysis** | Enhanced object & scene detection | ⭐⭐ Medium |
| 🗣️ **Voice Guidance** | "Turn left in 20 meters" | ⭐⭐ Medium |
| 🛍️ **Shop Details Page** | Tap to see menu, hours, photos, reviews | ⭐⭐ Medium |
| 🔔 **AR Offers/Promotions** | Floating discount badges in camera view | ⭐ Nice to Have |
| 🧑‍💼 **Business Dashboard** | Shop owners can update their info | ⭐ Nice to Have |
| 📊 **Analytics** | Track popular routes, shop visits | ⭐ Nice to Have |

### 🤖 Version 3.0 - AI & Personalization

| Feature | Description |
|---------|-------------|
| 🧠 **Personalized Recommendations** | ML-based suggestions based on history |
| 🎯 **AR Markers** | Shop-placed QR/ArUco markers for deals |
| 👥 **Social Features** | Share routes, favorite shops |
| 🌙 **Night Mode AR** | Enhanced low-light detection |
| 🏆 **Gamification** | Points for exploring, reviews |

---

## 🏗️ System Architecture

### High-Level Overview

```
┌─────────────────────────────────────────────────────────────┐
│                    ANDROID APP (Frontend)                    │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │   Camera +   │  │  AR Engine   │  │   OpenCV     │      │
│  │   Sensors    │  │  (ARCore)    │  │   Module     │      │
│  │              │  │              │  │              │      │
│  │ • GPS        │  │ • 3D render  │  │ • Scene      │      │
│  │ • Compass    │  │ • Tracking   │  │   analysis   │      │
│  │ • Gyroscope  │  │ • Anchors    │  │ • Object     │      │
│  └──────┬───────┘  └──────┬───────┘  │   detection  │      │
│         │                  │          └──────┬───────┘      │
│         └──────────────────┴─────────────────┘              │
│                            │                                 │
│                  ┌─────────▼─────────┐                      │
│                  │   AR Coordinate   │                      │
│                  │    Converter      │                      │
│                  └─────────┬─────────┘                      │
│                            │                                 │
│         ┌──────────────────┴──────────────────┐             │
│         │                                      │             │
│  ┌──────▼───────┐                   ┌─────────▼────────┐   │
│  │  Navigation  │                   │  Place Manager   │   │
│  │   Module     │                   │   (ViewModel)    │   │
│  │              │                   │                  │   │
│  │ • Route calc │                   │ • Fetch places   │   │
│  │ • AR arrows  │                   │ • Filter/sort    │   │
│  └──────────────┘                   │ • Cache data     │   │
│                                      └─────────┬────────┘   │
│                                                │             │
└────────────────────────────────────────────────┼─────────────┘
                                                 │
                         ┌───────────────────────▼──────────────────────┐
                         │           BACKEND (Spring Boot)              │
                         ├──────────────────────────────────────────────┤
                         │                                              │
                         │  ┌────────────┐      ┌─────────────┐       │
                         │  │  REST API  │      │   MySQL     │       │
                         │  │ Controllers│◄─────┤   Database  │       │
                         │  │            │      │             │       │
                         │  │ • Places   │      │ • Shops     │       │
                         │  │ • Routes   │      │ • Ratings   │       │
                         │  │ • Reviews  │      │ • Users     │       │
                         │  └──────┬─────┘      │ • Reviews   │       │
                         │         │            └─────────────┘       │
                         │         │                                   │
                         │  ┌──────▼─────┐      ┌─────────────┐       │
                         │  │  Business  │      │   Redis     │       │
                         │  │   Logic    │◄─────┤   Cache     │       │
                         │  │            │      │             │       │
                         │  │ • Geo calc │      │ • Location  │       │
                         │  │ • Ratings  │      │   queries   │       │
                         │  │ • Filters  │      │ • Fast reads│       │
                         │  └────────────┘      └─────────────┘       │
                         │                                              │
                         └──────────────────────────────────────────────┘
                                                │
                         ┌──────────────────────▼──────────────────────┐
                         │         EXTERNAL APIS                        │
                         ├──────────────────────────────────────────────┤
                         │  • Google Places API (shop data)             │
                         │  • Google Maps Directions API (routing)      │
                         │  • OpenStreetMap (alternative/backup)        │
                         │  • Text-to-Speech (voice navigation)         │
                         └──────────────────────────────────────────────┘
```

---

## 🛠️ Technology Stack

### 📱 Android Frontend

| Technology | Purpose | Why This Choice? |
|------------|---------|------------------|
| **Kotlin** | Primary programming language | Modern, concise, official Android language with null safety |
| **ARCore** | Augmented Reality foundation | Google's robust AR platform, excellent documentation |
| **Sceneform** | 3D rendering & AR objects | Simplified 3D model rendering on top of ARCore |
| **OpenCV Android SDK** | Computer vision | Scene analysis, object detection, image stabilization |
| **Jetpack Compose** | Modern UI framework | Declarative UI, less boilerplate than XML |
| **Retrofit** | REST API client | Type-safe HTTP requests, works great with coroutines |
| **Room Database** | Local caching | SQLite wrapper for offline support & fast queries |
| **Google Maps SDK** | Map integration | Industry standard for maps & location services |
| **CameraX** | Camera API | Modern camera implementation with easier lifecycle |
| **Kotlin Coroutines** | Async operations | Clean async/await syntax for network calls |

### 🌐 Backend

| Technology | Purpose | Why This Choice? |
|------------|---------|------------------|
| **Spring Boot** | REST API framework | You already know it! Robust, production-ready |
| **MySQL 8+** | Relational database | Spatial indexing for geo queries, reliable at scale |
| **Redis** | Caching layer | Ultra-fast location-based query caching |
| **JWT** | Authentication | Stateless, secure token-based authentication |
| **Spring Security** | Authorization | Role-based access (users, shop owners, admins) |
| **Docker** | Containerization | Easy deployment, consistent environments |
| **Swagger/OpenAPI** | API documentation | Auto-generated API docs for frontend team |

### 🗺️ Maps & Location Services

| Service | Purpose | Free Tier | Cost After |
|---------|---------|-----------|------------|
| **Google Places API** | Shop data & details | $200/month credit | $17 per 1000 requests |
| **Google Directions API** | Route calculation | Included in $200 credit | $5 per 1000 requests |
| **Google Maps SDK** | Map display | Free for most apps | Based on usage |
| **OpenStreetMap** | Backup/custom data | 100% Free | Always free |
| **Mapbox** | Alternative to Google | 50k requests/month | $5 per 1000 after |

**Recommendation:** Start with Google (free tier), add OpenStreetMap as fallback later.

---

## 🧮 How AR Works (Simplified)

### The Magic Behind AR Overlays

#### Step 1: Know Where You Are
```
User's Phone Sensors:
├── GPS → Latitude, Longitude (e.g., 28.6139, 77.2090)
├── Compass → Direction facing (e.g., 90° = East)
└── Gyroscope → Phone tilt/rotation
```

#### Step 2: Get Nearby Shops
```
API Request to Backend:
"Give me shops within 500m of (28.6139, 77.2090)"

Backend Response:
├── Sharma Medical: (28.6145, 77.2095) ⭐4.5
├── Burger Point: (28.6142, 77.2088) ⭐4.2
└── Book Store: (28.6148, 77.2100) ⭐4.7
```

#### Step 3: Convert GPS → AR Coordinates
```
For each shop:
1. Calculate bearing (direction from user to shop)
   → Sharma Medical is 45° from North
   
2. Calculate distance
   → 120 meters away
   
3. Convert to AR position (x, y, z)
   → x = sideways offset
   → y = height (usually fixed at 1.5m)
   → z = forward/backward (distance)
   
Result: Place shop label at (23.5, 1.5, -85.2) in AR space
```

#### Step 4: Render in Camera
```
ARCore draws:
├── Camera feed (real world)
└── 3D labels floating at calculated positions
    └── Automatically adjusts as user moves/rotates
```

**Key Formula (Simplified):**
```
bearing = angle from user to shop
distance = meters between points
relativeAngle = bearing - userHeading

AR_X = distance × sin(relativeAngle)
AR_Z = -distance × cos(relativeAngle)
AR_Y = 1.5 (fixed height)
```

---

## 🎨 OpenCV Integration - Why We Need It

### Problems OpenCV Solves

| Problem | Without OpenCV | With OpenCV |
|---------|---------------|-------------|
| **Jittery AR labels** | Labels shake as phone moves | Kalman filtering for smooth tracking |
| **No scene understanding** | Can't tell road from sky | Semantic segmentation (sky, road, buildings) |
| **Generic AR placement** | Labels float anywhere | Place labels on buildings intelligently |
| **Can't read shop signs** | Must rely on GPS data only | OCR to read actual shop signboards |
| **No marker support** | Only GPS-based AR | Detect QR/ArUco markers for precise placement |

### OpenCV Features We'll Use

#### 1. Scene Segmentation
```
Input: Camera frame
Output: Regions labeled as:
├── Sky (top 30% of screen)
├── Buildings (middle 40%)
└── Road (bottom 30%)

Use: Place AR labels in "building zone" only
```

#### 2. Shop Sign Detection
```
Process:
1. Detect rectangular shapes (potential signs)
2. Run OCR on detected regions
3. Match text with database shops
4. Pin AR label to detected sign

Benefit: Labels stick to actual shops, not floating randomly
```

#### 3. AR Stabilization (Kalman Filter)
```
Problem: Raw GPS/sensor data is noisy
Solution: Predict next position based on movement pattern

Result: Smooth, professional-looking AR overlays
```

#### 4. Marker-Based AR
```
Shop owners place QR/ArUco markers
OpenCV detects marker → triggers special AR content:
├── Exclusive deals
├── 3D product previews
└── Interactive menus
```

---

## 📊 Database Design

### Core Tables

#### Places Table
```
Stores: All shops, restaurants, services
Fields:
├── id (primary key)
├── name (e.g., "Sharma Medical")
├── category (e.g., "Pharmacy")
├── latitude, longitude (for geo queries)
├── address
├── phone, website
├── opening_hours (JSON)
└── created_at, updated_at

Special Index: Spatial index on (latitude, longitude)
→ Enables super-fast "find places within X meters" queries
```

#### Ratings Table
```
Stores: User reviews & ratings
Fields:
├── id
├── place_id (foreign key → places)
├── user_id (who rated)
├── rating (1.0 to 5.0)
├── review_text
└── created_at

Index: (place_id, rating) for fast average calculations
```

#### Users Table
```
Stores: App users
Fields:
├── id
├── username
├── email
├── password_hash (encrypted)
└── created_at
```

#### AR Markers Table *(Optional - for V2)*
```
Stores: Physical markers placed by shop owners
Fields:
├── id
├── place_id (which shop owns this marker)
├── marker_type (QR, ArUco, etc.)
├── marker_data (encoded info)
├── position (3D coordinates)
└── created_at
```

---

## 🚀 Development Roadmap

### Phase 1: Foundation (Weeks 1-2)
**Goal:** Basic app skeleton

- [ ] Setup Android Studio project with Kotlin
- [ ] Integrate ARCore (hello world AR scene)
- [ ] Implement GPS + Compass location tracking
- [ ] Create basic camera view with sensor overlay
- [ ] Setup Spring Boot backend with MySQL
- [ ] Create database schema & sample data
- [ ] Build REST API endpoint: `/api/places/nearby`

**Deliverable:** App that shows camera + user's GPS coordinates

---

### Phase 2: Core AR (Weeks 3-4)
**Goal:** Working AR overlays

- [ ] Implement GPS → AR coordinate conversion
- [ ] Create `PlaceNode` class (AR label for shops)
- [ ] Fetch nearby places from backend
- [ ] Display shops as floating AR labels
- [ ] Show: name, rating, distance
- [ ] Basic filtering (category, open now)

**Deliverable:** MVP - Camera shows nearby shops in AR!

---

### Phase 3: Enhanced UX (Weeks 5-6)
**Goal:** Make it beautiful & useful

- [ ] Design polished UI for AR labels
- [ ] Add shop details screen (tap label → full info)
- [ ] Implement rating/review system
- [ ] Add filters UI (sliders, checkboxes)
- [ ] Improve performance (caching, optimization)
- [ ] Add loading states & error handling

**Deliverable:** Polished MVP ready for testing

---

### Phase 4: Navigation (Weeks 7-8)
**Goal:** AR turn-by-turn directions

- [ ] Integrate Google Directions API
- [ ] Calculate routes between user & selected shop
- [ ] Create AR arrow components
- [ ] Place arrows on ground/road in AR
- [ ] Add voice navigation ("Turn right in 50m")
- [ ] Show ETA & remaining distance

**Deliverable:** Full AR navigation like Google Maps

---

### Phase 5: OpenCV Enhancement (Weeks 9-10)
**Goal:** Computer vision magic

- [ ] Setup OpenCV Android SDK
- [ ] Implement scene segmentation (sky/building/road)
- [ ] Build shop sign detector with OCR
- [ ] Add Kalman filter for smooth AR tracking
- [ ] Implement ArUco marker detection
- [ ] Smart label placement using CV insights

**Deliverable:** Professional-grade AR with CV intelligence

---

### Phase 6: Advanced Features (Weeks 11-12)
**Goal:** Startup-ready product

- [ ] User authentication & profiles
- [ ] Business owner dashboard (web panel)
- [ ] Shop owners can edit info, add photos
- [ ] Analytics dashboard (popular routes, visits)
- [ ] Push notifications for nearby deals
- [ ] Social features (share routes, favorite shops)

**Deliverable:** Complete product ready for launch

---

## 🎯 MVP Success Criteria

### Minimum Viable Product Checklist

A working MVP must have:

✅ **Core Functionality**
- [ ] App opens camera without crashing
- [ ] GPS location detected within 10m accuracy
- [ ] At least 5 nearby shops displayed in AR
- [ ] AR labels show: name, rating, distance
- [ ] Labels update as user rotates phone
- [ ] Tap label → shows shop details

✅ **Performance**
- [ ] App loads in < 3 seconds
- [ ] AR overlays render at 30+ FPS
- [ ] API calls complete in < 2 seconds
- [ ] Works on mid-range Android phones (not just flagships)

✅ **User Experience**
- [ ] Clear onboarding (explain AR features)
- [ ] Request permissions gracefully
- [ ] No crashes during 10-minute testing session
- [ ] Works in both portrait and landscape

✅ **Data Quality**
- [ ] At least 100 shops in database (for testing area)
- [ ] Ratings display correctly (1-5 stars)
- [ ] Distances accurate within 5%

---

## 💡 Why This Project Is Amazing

### For Your Resume
```
✅ Demonstrates cutting-edge tech (AR + AI)
✅ Full-stack (Android + Spring Boot + MySQL)
✅ Real-world problem solving
✅ Impressive demo video potential
✅ Shows mobile, backend, and CV skills
```

### For Learning
```
✅ Master ARCore (hot skill in job market)
✅ Learn computer vision with OpenCV
✅ Practice geo-spatial algorithms
✅ Understand sensor fusion (GPS + compass + gyro)
✅ Build production-quality REST APIs
```

### For Startup Potential
```
✅ Clear monetization paths:
   ├── Freemium (free basic, paid premium)
   ├── Business subscriptions (shop owners pay for analytics)
   ├── Sponsored placements
   └── Commission on in-app orders

✅ Scalable architecture
✅ Low initial costs (Google free tier)
✅ Viral potential (people will record & share AR experiences)
```

### For Interviews
**Common Interview Questions This Solves:**
- "Tell me about a complex project you built"
- "How do you handle real-time data?"
- "Explain a time you optimized performance"
- "What's the hardest technical challenge you faced?"

**Talking Points:**
- GPS coordinate to AR coordinate math
- Caching strategies for location queries
- OpenCV integration for scene understanding
- Handling multiple sensor inputs simultaneously

---

## 📚 Learning Resources

### ARCore & Android AR
- [ARCore Official Docs](https://developers.google.com/ar)
- [Sceneform Getting Started](https://developers.google.com/sceneform/develop)
- [Building AR Apps with Kotlin](https://www.udemy.com/course/arcore-android/)

### OpenCV
- [OpenCV Android Tutorials](https://docs.opencv.org/4.x/d9/df8/tutorial_android_dev_intro.html)
- [Computer Vision for Mobile](https://www.pyimagesearch.com/category/mobile/)

### Spring Boot + Geo Queries
- [Spatial Queries in MySQL](https://dev.mysql.com/doc/refman/8.0/en/spatial-analysis-functions.html)
- [Spring Boot REST API Best Practices](https://www.baeldung.com/rest-with-spring-series)

### Maps & Location
- [Google Places API Guide](https://developers.google.com/maps/documentation/places/web-service)
- [Coordinate Calculations](https://www.movable-type.co.uk/scripts/latlong.html)

---

## 🎬 Suggested Project Milestones for Team

### Week 1-2: Setup & Planning
**Team Meeting 1:**
- Review this README together
- Assign roles (Android dev, backend dev, CV specialist)
- Setup GitHub repo & project board
- Create development environment

### Week 3-4: MVP Sprint
**Team Meeting 2:**
- Review architecture decisions
- Integrate backend API with Android
- Demo: Camera showing GPS coordinates

### Week 5-6: AR Implementation
**Team Meeting 3:**
- Code review: AR coordinate conversion
- Test AR on multiple devices
- Demo: AR labels for nearby shops

### Week 7-8: Polish & Testing
**Team Meeting 4:**
- User testing with friends/family
- Fix bugs, optimize performance
- Prepare demo video

### Week 9+: Advanced Features
**Team Meeting 5:**
- Decide on V2 features (navigation vs OpenCV vs dashboard)
- Plan next sprint
- Consider beta launch

---

## 🤝 Team Roles Suggestion

### Role 1: Android Lead
**Responsibilities:**
- ARCore integration
- Camera & sensor handling
- UI/UX implementation
- OpenCV integration

**Skills Needed:** Kotlin, Android SDK, ARCore, OpenCV (optional)

---

### Role 2: Backend Lead
**Responsibilities:**
- Spring Boot API development
- Database design & optimization
- Redis caching
- Spatial queries

**Skills Needed:** Java/Spring Boot, MySQL, REST APIs

---

### Role 3: Full-Stack/Integration
**Responsibilities:**
- Connect Android ↔ Backend
- API testing
- Google Maps/Places integration
- Deployment (Docker, cloud hosting)

**Skills Needed:** Both Android & Backend basics

---

### Optional Role 4: CV/ML Specialist
**Responsibilities:**
- OpenCV scene analysis
- Shop sign detection
- Kalman filtering
- Marker detection

**Skills Needed:** Python/C++, OpenCV, computer vision theory

---

## 📝 Next Steps

### Immediate Actions (This Week)
1. **Team Meeting:** Review this README, ask questions
2. **Environment Setup:** Everyone installs Android Studio, MySQL, Redis
3. **GitHub Repo:** Create repo, add all team members
4. **Sample Data:** Collect 20-30 shops in your local area for testing
5. **API Keys:** Get Google Maps API key (free tier)

### First Sprint Goals (Week 1-2)
1. **Backend:** Working `/api/places/nearby` endpoint
2. **Android:** Camera view with GPS display
3. **Everyone:** Basic understanding of AR coordinate math

---

## 🎉 Final Thoughts

This project is **ambitious but totally achievable** with a focused team. The key is:

1. **Start with MVP** - Don't try to build everything at once
2. **Test frequently** - AR is visual, demo it every week
3. **Communicate** - AR math can be confusing, help each other
4. **Have fun!** - Seeing shops pop up in AR for the first time is magical ✨

**Remember:** Even Google Maps took years to perfect. Your MVP doesn't need to be perfect, it just needs to work and impress.

---

## 📞 Questions for First Team Meeting

1. Who wants to work on Android vs Backend?
2. Do we have any OpenCV experience in the team?
3. What area should we use for testing (local neighborhood)?
4. How many hours/week can each person commit?
5. Bi-weekly meetings or weekly?
6. Should we create a shared Google Drive for design mockups?

---

**Let's build something amazing! 🚀**

*Feel free to modify this README as the project evolves. Good luck, team!*
