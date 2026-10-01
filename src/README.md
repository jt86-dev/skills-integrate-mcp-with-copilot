# Mergington High School Activities API

A super simple FastAPI application that allows students to view and sign up for extracurricular activities.

## Features

- View all available extracurricular activities
- Sign up for activities

## Getting Started

1. Install the dependencies:

   ```
   pip install fastapi uvicorn
   ```

2. Configure a teacher login and session signing key in your shell. Do not commit these values:

   ```
   export ADMIN_USERNAME=teacher
   export ADMIN_PASSWORD='choose-a-strong-password'
   export SESSION_SECRET="$(python -c 'import secrets; print(secrets.token_hex(32))')"
   export ADMIN_COOKIE_SECURE=false
   ```

   `ADMIN_COOKIE_SECURE=false` is for local HTTP development only. Keep the default secure cookie setting when serving the app over HTTPS.

3. Run the application:

   ```
   uvicorn app:app --app-dir src --reload
   ```

4. Open your browser and go to:
   - Activities: http://localhost:8000/
   - API documentation: http://localhost:8000/docs
   - Alternative documentation: http://localhost:8000/redoc

## API Endpoints

| Method | Endpoint                                                          | Description                                                         |
| ------ | ----------------------------------------------------------------- | ------------------------------------------------------------------- |
| GET    | `/activities`                                                     | Get all activities with their details and current participant count |
| POST   | `/auth/login`                                                      | Log in as a teacher and receive a signed session cookie              |
| GET    | `/auth/session`                                                    | Check whether the current browser has a teacher session              |
| POST   | `/auth/logout`                                                     | End the current teacher session                                      |
| POST   | `/activities/{activity_name}/signup?email=student@mergington.edu` | Teacher-only student registration                                   |
| DELETE | `/activities/{activity_name}/unregister?email=student@mergington.edu` | Teacher-only student removal                                      |

Activity listings and participant rosters remain visible without signing in. Signup and unregister requests require a valid teacher session. Teacher credentials and the session signing key are provided through environment variables rather than checked-in files.

## Data Model

The application uses a simple data model with meaningful identifiers:

1. **Activities** - Uses activity name as identifier:

   - Description
   - Schedule
   - Maximum number of participants allowed
   - List of student emails who are signed up

2. **Students** - Uses email as identifier:
   - Name
   - Grade level

All data is stored in memory, which means data will be reset when the server restarts.
