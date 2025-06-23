# AI Challenge Generator - Backend

A FastAPI-based backend application that generates coding challenges using OpenAI's GPT model, with user authentication via Clerk and intelligent quota management.

## 🚀 Features

- **AI-Powered Challenge Generation**: Creates coding challenges at different difficulty levels using OpenAI GPT-3.5
- **Secure Authentication**: User authentication using Clerk with JWT token validation
- **Smart Quota Management**: Daily quota system with automatic resets (50 initial, 10 daily)
- **Challenge History**: Users can view their previously generated challenges
- **Webhook Integration**: Automatic user setup via Clerk webhooks
- **SQLite Database**: Persistent storage for challenges and user quotas

## 🛠️ Tech Stack

- **Backend**: FastAPI, Python 3.12
- **Database**: SQLite with SQLAlchemy ORM
- **Authentication**: Clerk Backend API
- **AI**: OpenAI GPT-3.5 Turbo
- **Package Management**: uv
- **Webhook Handling**: Svix

## 📁 Project Structure

```
backend/
├── src/
│   ├── __init__.py
│   ├── app.py                 # FastAPI application setup
│   ├── ai_generator.py        # OpenAI integration for challenge generation
│   ├── utils.py              # Authentication utilities
│   ├── database/
│   │   ├── models.py         # SQLAlchemy models
│   │   └── db.py             # Database operations
│   └── routes/
│       ├── __init__.py
│       ├── challenge.py      # Challenge-related endpoints
│       └── webhooks.py       # Clerk webhook handlers
├── server.py                 # Application entry point
├── database.db              # SQLite database
├── pyproject.toml           # Project dependencies
├── .env                     # Environment variables (not tracked)
└── .gitignore
```

## ⚙️ Environment Variables

Create a `.env` file in the root directory with the following variables:

```env
CLERK_SECRET_KEY=your_clerk_secret_key
JWT_KEY=your_jwt_key
CLERK_WEBHOOK_SECRET=your_webhook_secret
OPENAI_API_KEY=your_openai_api_key
```

## 🔧 Installation

1. **Clone the repository**

   ```bash
   git clone https://github.com/asheint/ai-challenge-generator-app-backend.git
   cd ai-challenge-generator-app-backend
   ```

2. **Install dependencies using uv**

   ```bash
   uv sync
   ```

3. **Set up environment variables**

   - Create a `.env` file in the root directory
   - Add your API keys and secrets as shown above

4. **Run the application**
   ```bash
   python server.py
   ```

The server will start on `http://localhost:8000`.

## 🔗 Frontend

This backend works with the AI Challenge Generator frontend:

- **Repository**: https://github.com/asheint/ai-challenge-generator-app
- **Frontend** handles user interface and authentication flow

## 📚 API Endpoints

### Challenge Management

#### **POST** `/api/generate-challenge`

Generate a new coding challenge

- **Authentication**: Required
- **Body**: `{"difficulty": "easy|medium|hard"}`
- **Response**: Challenge object with question, options, and metadata

#### **GET** `/api/my-history`

Get user's challenge history

- **Authentication**: Required
- **Response**: Array of user's previous challenges

#### **GET** `/api/quota`

Check remaining daily quota

- **Authentication**: Required
- **Response**: Current quota status and reset information

### Webhooks

#### **POST** `/webhooks/clerk`

Handle Clerk user creation events

- **Purpose**: Automatically sets up user quota when new users register
- **Security**: Verified using Clerk webhook secret

## 🗄️ Database Schema

### Challenge

- `id`: Primary key
- `difficulty`: Challenge difficulty level (easy/medium/hard)
- `date_created`: Creation timestamp
- `created_by`: User ID who created the challenge
- `title`: Challenge question
- `options`: JSON array of multiple choice options
- `correct_answer_id`: Index of correct answer (0-3)
- `explanation`: Detailed explanation of the correct answer

### ChallengeQuota

- `id`: Primary key
- `user_id`: Clerk user ID (unique)
- `quota_remaining`: Remaining daily quota
- `last_reset_date`: Last quota reset timestamp

## 🔐 Authentication

The application uses [Clerk](https://clerk.dev) for authentication:

- JWT token validation on protected endpoints
- Automatic user setup via webhooks
- Cross-origin support for frontend integration
- The [`authenticate_and_get_user_details`](src/utils.py) function handles token validation

## 📊 Quota System

- **New Users**: 50 initial challenges
- **Daily Reset**: 10 challenges every 24 hours
- **Automatic Management**: Quota resets are handled automatically via [`reset_quota_if_needed`](src/database/db.py)

## 🤖 AI Challenge Generation

Challenges are generated using OpenAI GPT-3.5 Turbo with:

- **Difficulty-based prompts**: Different complexity levels for easy/medium/hard
- **Structured output**: JSON format with question, options, correct answer, and explanation
- **Fallback system**: Default challenge if AI generation fails
- **Quality assurance**: Validation of required fields in AI response

## 🌐 CORS Configuration

Configured to allow requests from:

- `localhost:5173` (Vite dev server)
- `localhost:5174` (Alternative frontend port)
- Full CORS support for development

## 🛡️ Error Handling

Comprehensive error handling for:

- **401**: Authentication failures
- **429**: Quota exhaustion
- **400**: Invalid requests
- **500**: Server errors

## 🚀 Development

### Key Dependencies

- **FastAPI**: Modern web framework for APIs
- **SQLAlchemy**: Database ORM
- **Clerk Backend API**: Authentication service
- **OpenAI**: AI challenge generation
- **Svix**: Webhook verification
- **python-dotenv**: Environment variable management

### Running in Development

```bash
python server.py
```

The application will reload automatically on code changes when using uvicorn with reload flag.

## ⭐ Support

If you found this project helpful, please star the repository!

[![GitHub stars](https://img.shields.io/github/stars/asheint/ai-challenge-generator-app-backend.svg?style=social&label=Star)](https://github.com/asheint/ai-challenge-generator-app-backend)

## ☕ Buy Me A Coffee

Support the development of this project:

[![Buy Me A Coffee](https://img.shields.io/badge/Buy%20Me%20A%20Coffee-support-yellow.svg)](https://buymeacoffee.com/asheint)

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

---

**Related Projects:**

- [Frontend Repository](https://github.com/asheint/ai-challenge-generator-app) - React frontend for this application
