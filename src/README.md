# Mergington High School Activities API

A super simple FastAPI application that allows students to view and sign up for extracurricular activities.

## Features

- View all available extracurricular activities
- View registered students
- Teacher login to register or unregister students

## Getting Started

1. Install the dependencies:

   ```
   pip install -r ../requirements.txt
   ```

2. Run the application:

   ```
   python app.py
   ```

3. Open your browser and go to:
   - API documentation: http://localhost:8000/docs
   - Alternative documentation: http://localhost:8000/redoc

## API Endpoints

| Method | Endpoint                                                          | Description                                                         |
| ------ | ----------------------------------------------------------------- | ------------------------------------------------------------------- |
| GET    | `/activities`                                                     | Get all activities with their details and current participant count |
| POST   | `/auth/login`                                                     | Sign in as a teacher                                                |
| GET    | `/auth/session`                                                   | Check the current teacher session                                   |
| POST   | `/auth/logout`                                                    | Sign out                                                            |
| POST   | `/activities/{activity_name}/signup?email=student@mergington.edu` | Sign up for an activity                                             |
| DELETE | `/activities/{activity_name}/unregister?email=student@mergington.edu` | Unregister a student (teacher only)                             |

Signup and unregister requests require a teacher session. Add credentials to the `teachers` object in `teachers.json`; passwords are stored as salted PBKDF2-SHA256 hashes, never as plaintext. Generate a credential entry with:

```sh
python -c 'import getpass,hashlib,json,secrets; password=getpass.getpass("Teacher password: "); salt=secrets.token_hex(16); print(json.dumps({"salt":salt,"password_hash":hashlib.pbkdf2_hmac("sha256",password.encode(),salt.encode(),310000).hex()}))'
```

Use the output as the value for a teacher username in the JSON file. Set `SESSION_SECRET` to a stable random secret when deploying, and set `COOKIE_SECURE=true` when serving over HTTPS.

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
