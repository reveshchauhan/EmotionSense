# Emotion Recognition System

## Overview

This is a full-stack emotion recognition web application that analyzes human facial expressions from images or webcam input. The system uses TensorFlow.js with Face-API for client-side face detection and emotion analysis, providing real-time feedback through interactive charts and visualizations. Users can upload images or capture photos via webcam to detect emotions like happiness, sadness, anger, surprise, and neutral expressions. The application includes a dashboard for viewing historical analysis data and trends over time.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture
The client-side is built with React 18 and TypeScript, using Vite as the build tool. The UI framework is based on Radix UI components styled with Tailwind CSS and follows a dark theme design pattern. Key architectural decisions include:

- **Component-based architecture**: Modular React components with clear separation of concerns
- **State management**: React Query for server state management and built-in React state for local UI state
- **Routing**: Wouter for lightweight client-side routing
- **AI processing**: Face-API.js runs entirely in the browser for privacy-focused emotion detection
- **Responsive design**: Mobile-first approach using Tailwind's responsive utilities

The face detection models are loaded from a CDN to avoid bundling large ML models, improving initial load times while maintaining functionality.

### Backend Architecture  
The server uses Express.js with TypeScript in ESM format. The architecture follows a RESTful API pattern with the following key components:

- **Express server**: Handles HTTP requests and serves the React application in production
- **API routes**: RESTful endpoints for emotion data persistence and retrieval
- **File handling**: Processes base64 image data from client uploads and webcam captures
- **Server-side validation**: Uses Zod schemas for request validation

The server also includes Face-API.js for server-side processing as a fallback, though the primary emotion detection happens on the client.

### Database Design
Uses PostgreSQL with Drizzle ORM for type-safe database operations. The schema includes:

- **emotion_detections**: Stores detection metadata (source, timestamp, optional image reference)
- **emotions**: Stores individual emotion scores linked to detections via foreign key

This normalized approach allows for efficient querying of historical data and supports future analytics features. The database design prioritizes privacy by not storing actual image data, only metadata and results.

### Data Processing Pipeline
Emotion detection follows this flow:

1. **Image acquisition**: Either file upload or webcam capture
2. **Client-side processing**: Face-API.js detects faces and analyzes emotions
3. **Result formatting**: Raw emotion scores are processed into user-friendly percentages with color coding
4. **Data persistence**: Results are sent to the server and stored in PostgreSQL
5. **Visualization**: Charts and historical views are generated from stored data

The system prioritizes client-side processing to protect user privacy - images never leave the browser during analysis.

### Security and Privacy
- **No image storage**: Only emotion analysis results are persisted, never the actual images
- **Client-side processing**: Face detection happens in the browser to protect user privacy
- **Input validation**: Server-side validation prevents malicious data submission
- **CORS configuration**: Appropriate cross-origin policies for API endpoints

## External Dependencies

### Core Technologies
- **React 18**: Frontend framework with TypeScript support
- **Express.js**: Node.js web server framework
- **PostgreSQL**: Primary database via Neon Database service
- **Drizzle ORM**: Type-safe database operations and migrations

### AI/ML Libraries
- **Face-API.js**: Browser-based face detection and emotion recognition
- **TensorFlow.js Node**: Server-side ML processing capabilities
- **Canvas**: Required for server-side image processing

### UI/UX Libraries
- **Radix UI**: Accessible component primitives
- **Tailwind CSS**: Utility-first CSS framework
- **Recharts**: Interactive charts for data visualization
- **React Webcam**: Webcam integration component
- **React Query**: Server state management and caching

### Development Tools
- **Vite**: Fast build tool and development server
- **TypeScript**: Type safety across the entire stack
- **Drizzle Kit**: Database schema management and migrations
- **ESBuild**: Production bundling for the server

### Cloud Services
- **Neon Database**: Managed PostgreSQL hosting
- **Face-API.js CDN**: Remote model loading for ML capabilities

The architecture balances performance, privacy, and user experience by keeping sensitive AI processing on the client while maintaining a robust backend for data persistence and analytics.