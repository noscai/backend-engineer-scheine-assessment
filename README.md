### Backend-Only Assessment Test: Database Design, Validation, and PDF Generation for Multiple "Schein" Types with Doctor and Patient Information

**Objective:**  
You are tasked with building a backend system that handles the creation, validation, and management of over 90 different types of "Scheine" commonly used in German medical practices. Each "Schein" type has its own unique form fields and structure. The system should validate the form data based on the selected "Schein" type, store it in a PostgreSQL database, and generate a PDF based on the corresponding template.

The API should return the generated PDF as a base64-encoded string in the response.

### Technology Stack:
- **Backend**:
  - **ExpressJS**
  - **TypeORM** with PostgreSQL
  - **NodeJS PDF generator** (e.g., PDFKit or another of your choice)

### PDF Templates:
You are provided with several example templates:
1. **Mustersammlung** (General medical certificate) [file: Mustersammlung (1).pdf]
2. **Zeugnis über den mutmaßlichen Tag der Entbindung** (Certificate for estimated date of childbirth) [file: Zeugnis_über_den_mutmaßlichen_tag_der_entbindung (1).pdf]
3. **Verordnung einer Krankenbeförderung** (Transportation order for medical purposes) [file: Verordnung_einer_krankenbeforderung (1).pdf]

Each of these templates corresponds to a different "Schein" type and requires specific fields to be validated and rendered in the PDF.

### Task Description:

#### 1. **Backend Development (ExpressJS, TypeORM, PostgreSQL)**

1. **Database Design**:
   - **Schema for Multiple "Schein" Types**:
     - Design a PostgreSQL database schema using TypeORM that can handle different "Schein" types.
     - The schema should support:
       - **Common Fields**: `patient_id`, `doctor_id`, `date_of_issue`.
       - **Type-Specific Fields**: Fields unique to each "Schein" type (e.g., transportation details, diagnosis).
     - **Patient and Doctor Tables**:
       - Create separate tables for `patients` and `doctors` to store their respective information.
       - **Patients**: `id`, `name`, `date_of_birth`, `insurance_number`, etc.
       - **Doctors**: `id`, `name`, `doctor_number`, `signature`, `medical_practice_number`, etc.
       - When creating a "Schein", only the `patient_id` and `doctor_id` should be provided, and their related data will be retrieved automatically from the database.

2. **TypeORM Migrations**:
   - Implement TypeORM migrations to create the database schema, including the `patients`, `doctors`, and `scheine` tables.
   - The `scheine` table should include the specific type of "Schein" and a JSON or specific column structure to handle type-specific fields.

#### 2. **API Development**:
   - **Create Schein**:
     - Develop an API endpoint (`POST /scheine`) that accepts input data for any "Schein" type.
     - Input data should include `patient_id`, `doctor_id`, and any type-specific fields required for the "Schein" type.
     - The API should retrieve patient and doctor details from the `patients` and `doctors` tables based on the provided `patient_id` and `doctor_id`.
     - The API should validate the form data based on the specific "Schein" type provided in the request (e.g., validate transportation details for **Verordnung einer Krankenbeförderung**).

   - **Preview Schein (Return as Base64 PDF)**:
     - Create an endpoint (`GET /scheine/:id/preview`) that retrieves the "Schein" data, including the patient and doctor information, and generates a PDF of the "Schein" using the correct template.
     - The API should fetch the form data, patient, and doctor details from the database, generate a PDF using a Node.js PDF library (e.g., PDFKit), and return the generated PDF as a base64-encoded string in the response.

   - **List Scheine**:
     - Create an endpoint (`GET /scheine`) to list all "Scheine" stored in the database, with filtering options based on patient, doctor, and date.

#### 3. **PDF Generation**:
   - Use a Node.js PDF generator to generate the PDF for each "Schein" type.
   - The generated PDF should match the provided template, with all input fields (e.g., patient name, transportation details, date of birth) placed in the correct positions.
   - Retrieve patient and doctor details from the `patients` and `doctors` tables and insert them into the PDF.
   - Return the generated PDF as a base64-encoded string.

#### 4. **Data Validation**:
   - Ensure that each "Schein" type has its own validation rules. For example:
     - **Mustersammlung**: Validate fields such as patient name (from `patients`), diagnosis, and doctor's signature (from `doctors`).
     - **Zeugnis über den mutmaßlichen Tag der Entbindung**: Validate fields related to childbirth, including expected delivery date, examination date, and patient’s insurance information (from `patients`).
     - **Verordnung einer Krankenbeförderung**: Validate fields such as type of transportation required, medical necessity, and patient mobility issues.
   - Use appropriate error handling for missing or invalid input, returning clear error messages to the client.

### Additional Requirements:

1. **Validation System**:
   - Implement a flexible validation system that allows easy addition of new "Schein" types and their specific validation rules.
   - Each "Schein" type should have unique constraints based on its specific fields, and the system should validate the form data accordingly before storing it in the database.

2. **PDF Templates**:
   - Generate the preview for the appropriate "Schein" type based on the provided PDF template (e.g., `Mustersammlung.pdf`, `Verordnung_einer_krankenbeforderung.pdf`, or `Zeugnis_über_den_mutmaßlichen_tag_der_entbindung.pdf`).
   - Ensure that the generated PDF correctly reflects the input fields in the correct locations for each type of "Schein."

3. **Documentation**:
   - Provide a clear README explaining how to set up the project and how the API works.
   - Include examples of requests and responses for creating, validating, and previewing "Scheine."

### Instructions:

1. **Project Setup**:
   - Set up an ExpressJS server.
   - Configure TypeORM with PostgreSQL to store form data, and implement the schema using migrations.
   - Install necessary libraries (e.g., `pdfkit`, `axios`).

2. **Submission**:
   - Provide a link to a GitHub repository containing your implementation.
   - Include a README with setup instructions and any design decisions you made.
   - Provide example API requests and responses in the README.

### Evaluation Criteria:

- **Database Design**: Is the schema well-structured, scalable, and designed to handle various types of "Scheine" and their unique fields while retrieving related information from `patients` and `doctors` tables?
- **Validation System**: Does the validation system accurately enforce rules for each "Schein" type, and is it extendable for new types?
- **API Design**: Are the API endpoints functional and appropriate for creating, storing, and previewing "Scheine"?
- **PDF Accuracy**: Does the generated PDF match the provided template? Are the form fields correctly placed using the retrieved data from `patients` and `doctors` tables?
- **Code Quality**: Is the code clean, modular, and well-structured?
- **Error Handling**: Does the system handle missing or invalid inputs gracefully with appropriate error messages?
- **Documentation**: Is the project well-documented with clear setup instructions and examples?

---

### Example of Request and Response:

#### Request (for "Verordnung einer Krankenbeförderung"):
```json
POST /scheine
{
  "type": "Verordnung_einer_Krankenbeförderung",
  "patient_id": 1,
  "doctor_id": 3,
  "transportation": {
    "type": "KTW",
    "medical_necessity": "required for wheelchair"
  },
  "date": "2024-01-10"
}
```

#### Response:
```json
{
  "id": 1,
  "type": "Verordnung_einer_Krankenbeförderung",
  "patient": {
    "id": 1,
    "name": "John Doe",
    "date_of_birth": "1990-01-01",
    "insurance_number": "123456789"
  },
  "doctor": {
    "id": 3,
    "name": "Dr. Smith",
    "doctor_number": "DR987",
    "signature": "digital_signature_string"
  },
  "transportation": {
    "type": "KTW",
    "medical_necessity": "required for wheelchair"
  },
  "created_at": "2024-01-10T10:00:00Z",
  "pdf_base64": "JVBERi0xLjMKJcfsj6IKNSAwIG9iago8PAovTGVuZ3..."
}
```
