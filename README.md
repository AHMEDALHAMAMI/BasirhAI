# BasirhAI 🩺🤖

**BasirhAI** is an AI-powered health management backend built with **Java Spring Boot**.  
The system collects user health information and vital-sign measurements, analyzes health trends and potential risks, provides evidence-based health insights, and uses **Anthropic Claude Haiku** to generate personalized nutrition and exercise plans.


---

## 🚀 Features

### 👤 User Management
- Create, retrieve, update, and delete users.
- User profile information includes:
  - Full name
  - Email
  - Phone
  - Date of birth
  - Gender
  - WhatsApp opt-in
  - Role

### 🩺 Health Profile
- Store and manage a user's health profile.
- Track:
  - Height
  - Activity level
  - Health conditions
  - Exercise risk
- Supported activity levels:
  - `SEDENTARY`
  - `LIGHT`
  - `MODERATE`
  - `ACTIVE`

### ❤️ Vital Signs
The system supports:
- Blood pressure
- Glucose
- Weight
- Waist circumference
- Heart rate

Vital records include measurement values, units, timestamps, and automatically calculated health flags.

### 📊 Health Analysis
BasirhAI provides:
- Health risk assessment
- Health insights
- Latest health summary
- Vital-sign trend comparison
- Health assessment readiness
- Doctor follow-up recommendations
- Doctor appointment eligibility
- Health mission recommendations

The health-insight system compares the latest and previous measurements to calculate changes and trends.

### 🧠 AI-Powered Personalized Plans
The application integrates with **Anthropic Claude Haiku** to generate personalized:
- Nutrition plans
- Exercise plans
- Meal alternatives/swaps
- Exercise adaptations

AI recommendations are generated using the user's available health profile and vital-sign data.

### 📚 Evidence-Based Recommendations
The system maintains an evidence database containing health references and recommendations from sources such as:
- American Diabetes Association (ADA)
- American Heart Association (AHA)
- American College of Cardiology (ACC)
- World Health Organization (WHO)
- PubMed research

Relevant evidence is selected based on the user's health conditions and latest measurements before generating recommendations.

### 📱 WhatsApp Notifications
The backend integrates with **Twilio WhatsApp** to support doctor appointment follow-up notifications for users who have opted in.

---

## 🏗️ Architecture

The project follows a layered Spring Boot architecture:

```text
src/main/java/com/example/rafeeq/
│
├── Advice/
│   └── ControllerAdvice.java
│
├── Api/
│   ├── ApiException.java
│   └── ApiResponse.java
│
├── Config/
│   ├── AnthropicConfig.java
│   └── EvidenceDataInitializer.java
│
├── Controller/
│   ├── AIController.java
│   ├── HealthAnalysisController.java
│   ├── HealthContentController.java
│   ├── HealthProfileController.java
│   ├── UserController.java
│   └── VitalSignController.java
│
├── DTO/
│   ├── AIPlanResponseDTO.java
│   ├── HealthInsightResponseDTO.java
│   ├── HealthRiskResponseDTO.java
│   ├── HealthSummaryResponseDTO.java
│   ├── HealthProfileRequestDTO.java
│   ├── VitalSignRequestDTO.java
│   └── ...
│
├── Model/
│   ├── User.java
│   ├── HealthProfile.java
│   ├── VitalSign.java
│   ├── NutritionPlan.java
│   ├── ExercisePlan.java
│   └── EvidenceReference.java
│
├── Repository/
│   ├── UserRepository.java
│   ├── HealthProfileRepository.java
│   ├── VitalSignRepository.java
│   ├── NutritionPlanRepository.java
│   ├── ExercisePlanRepository.java
│   └── EvidenceReferenceRepository.java
│
├── Service/
│   ├── AIService.java
│   ├── AnthropicService.java
│   ├── EvidenceService.java
│   ├── HealthAnalysisService.java
│   ├── HealthContentService.java
│   ├── HealthProfileService.java
│   ├── UserService.java
│   ├── VitalSignService.java
│   └── TwilioService.java
│
└── RafeeqApplication.java
```

### Request Flow

```text
Client / Postman
       │
       ▼
   Controller
       │
       ▼
     DTO + Validation
       │
       ▼
     Service
       │
       ├──────────────► Repository ─────► MySQL
       │
       ├──────────────► Evidence Service
       │
       ├──────────────► Anthropic Claude
       │
       └──────────────► Twilio WhatsApp
       │
       ▼
    Response DTO
```

---

## 🛠️ Technologies

| Technology | Purpose |
|---|---|
| Java 17 | Programming language |
| Spring Boot 3.3.5 | Backend framework |
| Spring Web | REST APIs |
| Spring Data JPA | Database access |
| Spring Validation | Request validation |
| MySQL | Relational database |
| Lombok | Boilerplate reduction |
| Maven | Dependency management |
| Anthropic Claude Haiku | AI-generated health plans |
| Twilio | WhatsApp notifications |
| Postman | API testing |

---

## 🔌 API Endpoints

Base URL:

```text
http://localhost:8080/v1/api
```

### User APIs

| Method | Endpoint | Description |
|---|---|---|
| GET | `/users/get` | Get all users |
| GET | `/users/get/{id}` | Get user by ID |
| POST | `/users/add` | Add user |
| PUT | `/users/update/{id}` | Update user |
| DELETE | `/users/delete/{id}` | Delete user |

### Health Profile APIs

| Method | Endpoint | Description |
|---|---|---|
| GET | `/health-profiles/get` | Get all health profiles |
| GET | `/health-profiles/get/{id}` | Get health profile by ID |
| GET | `/health-profiles/get-by-user/{userId}` | Get profile by user |
| POST | `/health-profiles/add` | Add health profile |
| PUT | `/health-profiles/update/{id}` | Update health profile |
| DELETE | `/health-profiles/delete/{id}` | Delete health profile |

### Vital Sign APIs

| Method | Endpoint | Description |
|---|---|---|
| GET | `/vital-signs/get` | Get all vital signs |
| GET | `/vital-signs/get/{id}` | Get vital sign by ID |
| GET | `/vital-signs/get-by-user/{userId}` | Get user's vital signs |
| POST | `/vital-signs/add` | Add vital sign |
| PUT | `/vital-signs/update/{id}` | Update vital sign |
| DELETE | `/vital-signs/delete/{id}` | Delete vital sign |

### Health Analysis APIs

| Method | Endpoint | Description |
|---|---|---|
| GET | `/health-risks/{userId}` | Analyze potential health risks |
| GET | `/health-insights/{userId}` | Compare recent vital measurements |
| GET | `/health-summary/{userId}` | Get health dashboard summary |
| GET | `/health-assessment/readiness/{userId}` | Check assessment readiness |
| GET | `/doctor-follow-up/{userId}` | Get doctor follow-up recommendations |
| GET | `/doctor-appointments/eligibility/{userId}` | Check appointment eligibility |
| GET | `/health-mission/{userId}` | Get personalized health mission |
| POST | `/doctor-appointments/send-whatsapp/{userId}` | Send appointment follow-up via WhatsApp |

### AI APIs

| Method | Endpoint | Description |
|---|---|---|
| POST | `/ai/generate-plan/{userId}` | Generate nutrition and exercise plans |
| POST | `/ai/meal-swap/{userId}` | Generate a meal alternative |
| POST | `/ai/exercise-adapt/{userId}` | Adapt an exercise recommendation |

### Health Content

| Method | Endpoint | Description |
|---|---|---|
| GET | `/health-content/get` | Retrieve health education content |

---

## 🧪 Postman

A complete Postman collection is included in the repository:

```text
Rafeeq_Health.postman_collection.json
```

Import this file into Postman to test the API endpoints.

Recommended testing order:

```text
1. Create User
2. Create Health Profile
3. Add Vital Signs
4. Add a second measurement for comparison
5. Get Health Summary
6. Get Health Insights
7. Get Health Risks
8. Check Assessment Readiness
9. Check Doctor Follow-Up
10. Check Appointment Eligibility
11. Generate AI Plan
12. Test Meal Swap / Exercise Adaptation
13. Test WhatsApp notification (if Twilio is configured)
```

---

## ⚙️ Requirements

Before running the project, install:

- Java 17+
- Maven
- MySQL 8+
- Git
- Postman (recommended)

---

## 🗄️ Database Setup

Create the database in MySQL:

```sql
CREATE DATABASE rafeeq_db;
```

The application is configured to use:

```text
Database: rafeeq_db
Username: root
Password: empty by default
```

Update the database configuration in:

```text
src/main/resources/application.properties
```

Example:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/rafeeq_db
spring.datasource.username=root
spring.datasource.password=
```

The project currently uses:

```properties
spring.jpa.hibernate.ddl-auto=update
```

so Hibernate updates the database schema automatically.

---

## 🔐 Environment Variables

Do **not** commit API keys, tokens, or other secrets to GitHub.

The project uses environment variables for external services.

### Anthropic

```text
ANTHROPIC_API_KEY
```

### Twilio

```text
TWILIO_ACCOUNT_SID
TWILIO_AUTH_TOKEN
TWILIO_WHATSAPP_CONTENT_SID
TWILIO_WHATSAPP_FROM
```

### Appointment Booking

```text
APPOINTMENT_BOOKING_URL
```

Example:

```text
ANTHROPIC_API_KEY=your_anthropic_api_key
TWILIO_ACCOUNT_SID=your_twilio_account_sid
TWILIO_AUTH_TOKEN=your_twilio_auth_token
TWILIO_WHATSAPP_CONTENT_SID=your_content_sid
TWILIO_WHATSAPP_FROM=your_whatsapp_sender
APPOINTMENT_BOOKING_URL=http://localhost:8080/v1/api/doctor-appointments/book
```

The application reads the Anthropic model configuration as:

```properties
anthropic.api.model=claude-haiku-4-5-20251001
```

---

## ▶️ Running the Project

### 1. Clone the repository

```bash
git clone https://github.com/AHMEDALHAMAMI/BasirhAI.git
cd BasirhAI
```

### 2. Configure MySQL

Create:

```text
rafeeq_db
```

and update your database credentials if required.

### 3. Configure environment variables

Set the required Anthropic and Twilio environment variables.

### 4. Run with Maven

```bash
mvn clean install
mvn spring-boot:run
```

Or run:

```bash
./mvnw spring-boot:run
```

### 5. Test the API

The API will be available at:

```text
http://localhost:8080/v1/api
```

---

## 🧠 AI Recommendation Flow

The AI planning process follows this general flow:

```text
User
 │
 ▼
Health Profile
 │
 ▼
Latest Vital Signs
 │
 ▼
Health Analysis
 │
 ├── Blood Pressure
 ├── Glucose
 ├── Weight
 ├── Waist
 └── Heart Rate
 │
 ▼
Relevant Evidence
 │
 ├── Diabetes evidence
 ├── Hypertension evidence
 ├── Nutrition evidence
 └── Exercise evidence
 │
 ▼
Anthropic Claude Haiku
 │
 ▼
Personalized Nutrition + Exercise Plan
 │
 ▼
Stored Plan / API Response
```

This approach combines structured health data with an evidence layer before requesting AI-generated recommendations.

---

## 📈 Health Insight Logic

For supported measurements, the system retrieves the latest two readings.

For example:

```text
Previous glucose: 140
Latest glucose:   155

Change = +15
Change % = +10.71%
Trend = INCREASED
```

For blood pressure, systolic and diastolic values are compared independently.

Possible trends include:

```text
INCREASED
DECREASED
UNCHANGED
MIXED
INSUFFICIENT_DATA
```

---

## 📚 Evidence Layer

The application initializes evidence references when the application starts.

Evidence topics include:

```text
DIABETES_NUTRITION
DIABETES_EXERCISE
HYPERTENSION
HYPERTENSION_NUTRITION
GENERAL_NUTRITION
GENERAL_EXERCISE
```

This allows the application to select relevant references based on the user's health conditions and latest measurements.

---

## 🛡️ Validation & Error Handling

The project uses Jakarta Bean Validation for request validation.

Examples include:

```text
@NotBlank
@NotNull
@Email
@Size
@Pattern
@Positive
@DecimalMin
@DecimalMax
```

Centralized API error handling is provided through:

```text
ControllerAdvice.java
```

This keeps validation and business exceptions consistent across the API.

---

## 📁 Project Structure

```text
BasirhAI/
├── src/
│   └── main/
│       ├── java/
│       │   └── com/example/rafeeq/
│       └── resources/
│           └── application.properties
│
├── Rafeeq_Health.postman_collection.json
├── pom.xml
├── .gitignore
└── README.md
```

---

## 🔒 Security Note

This project is intended as a health-management and decision-support backend.

AI-generated recommendations are **not a substitute for diagnosis, treatment, or professional medical advice**. Users with urgent or severe symptoms should seek appropriate medical care.

API keys, authentication tokens, database passwords, and other secrets should always be stored outside source control.

---

## 🔮 Future Improvements

Potential future improvements include:

- User authentication and authorization with Spring Security
- JWT-based authentication
- Doctor accounts and appointment management
- More advanced health trend analytics
- Additional evidence sources
- Multilingual Arabic/English AI responses
- Health dashboards and visualization
- Notification scheduling
- More wearable/device integrations
- Automated health-report generation

---

## 👨‍💻 Author

**Ahmed Al-Hamami**

GitHub:

```text
https://github.com/AHMEDALHAMAMI
```

Repository:

```text
https://github.com/AHMEDALHAMAMI/BasirhAI
```

---

## ⭐ Project Summary

**BasirhAI** combines:

```text
Spring Boot
     +
MySQL
     +
Health Data
     +
Evidence-Based Knowledge
     +
Anthropic Claude
     +
Twilio WhatsApp
     =
AI-Powered Health Management Backend
```

Built to transform raw health measurements into understandable insights, risk indicators, personalized recommendations, and actionable follow-up guidance.
