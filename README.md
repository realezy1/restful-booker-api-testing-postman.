# Restful Booker API Testing with Postman

## Project Overview

This project focuses on API testing of the Restful Booker API using Postman. The purpose is to validate API functionality, verify response data, test authentication, identify potential defects, and evaluate how the API handles valid and invalid inputs.

Testing activities include positive testing, negative testing, response validation, authentication testing, and data verification.

**API:** Restful Booker  
**Base URL:** https://restful-booker.herokuapp.com  
**API Documentation:** https://restful-booker.herokuapp.com/apidoc/index.html  
**Testing Tool:** Postman  
**Programming Language:** JavaScript  
**Project Type:** API Testing

---

## Objectives

- Understand and test REST API functionality.
- Validate HTTP methods and status codes.
- Verify JSON response structures and data types.
- Test authentication and authorization.
- Perform positive and negative testing.
- Identify and document potential API defects.
- Practice writing test scripts and assertions using Postman.

---

## Tools & Technologies

| Tools | Purpose |
|---|---|
| Postman | API testing and request execution |
| JavaScript | Writing test scripts and assertions |
| REST API | API communication and testing |
| JSON | Request and response data format |
| GitHub | Project documentation and version control |

---
---

# API Test Cases

The following test cases cover the main API functionalities tested in this project.

| Test Case ID | Module | Method | Test Scenario |
|---|---|---|---|
| HC-001 | Health Check | GET | Verify API availability |
| AUTH-001 | Authentication | POST | Create token with valid credentials |
| AUTH-002 | Authentication | POST | Create token with invalid password |
| AUTH-003 | Authentication | POST | Create token with missing credentials |
| GET-001 | Get Booking | GET | Retrieve all booking IDs |
| POST-001 | Create Booking | POST | Create booking with valid data |
| POST-002 | Create Booking | POST | Create booking with missing data |
| POST-003 | Create Booking | POST | Create booking with invalid numeric data type |
| POST-004 | Create Booking | POST | Create booking with invalid Boolean data type |
| POST-005 | Create Booking | POST | Create booking with invalid date format |
| POST-006 | Create Booking | POST | Create booking with invalid date range |
| PUT-001 | Update Booking | PUT | Update booking with valid data |
| PATCH-001 | Update Booking | PATCH | Update booking with partial data |
| PATCH-002 | Update Booking | PATCH | Update booking without authentication token |
| PATCH-003 | Update Booking | GET | Verify booking data after unauthorized update |
| PATCH-004 | Update Booking | PATCH | Update booking using invalid booking ID |
| PATCH-005 | Update Booking | PATCH | Update booking with invalid numeric data type |

---

# API Execution Report

This section summarizes the observed results from the API testing activities.

## Execution Summary

| Module | Total Test Cases | Execution Notes |
|---|---:|---|
| Health Check | 1 | PASS |
| Authentication | 3 | All PASS |
| Get Booking | 1 | PASS |
| Create Booking | 6 | Successful and potential validation issues observed |
| Update Booking | 6 | PASS and FAIL results observed |

## Detailed Execution Results

| Test Case ID | Actual Result | Status |
|---|---|---|
| HC-001 | HTTP 201 Created | PASS |
| AUTH-001 | HTTP 200, token successfully generated and saved | PASS |
| AUTH-002 | HTTP 200, Bad credentials | PASS |
| AUTH-003 | HTTP 200, Bad credentials | PASS |
| GET-001 | HTTP 200, booking ID array returned | PASS |
| POST-001 | HTTP 200, booking successfully created | PASS |
| POST-002 | HTTP 500, Internal Server Error | Potential Issue |
| POST-003 | HTTP 200, totalprice returned as null | Potential Issue |
| POST-004 | HTTP 200, invalid Boolean input converted to true | Potential Issue |
| POST-005 | HTTP 200, malformed date values returned | Potential Issue |
| POST-006 | HTTP 200, invalid date range accepted | Potential Issue |
| PUT-001 | HTTP 200, booking successfully updated | PASS |
| PATCH-001 | HTTP 200, partial update successful | PASS |
| PATCH-002 | HTTP 403, Forbidden | PASS |
| PATCH-003 | GET confirmed booking data remained unchanged after unauthorized update | PASS |
| PATCH-004 | HTTP 405 instead of expected HTTP 404 | FAIL |
| PATCH-005 | HTTP 200, totalprice returned and persisted as null | FAIL |

**Note:** Potential issues are observations that require confirmation against the API requirements. They are not automatically confirmed defects.

---

# Defect Log

The following observations were identified during negative testing.

| Defect ID | Related Test Case | Description | Actual Result | Initial Severity |
|---|---|---|---|---|
| DEF-001 | POST-002 | Missing booking data handling | HTTP 500 Internal Server Error | Medium |
| DEF-002 | POST-003 | Invalid numeric data type accepted | totalprice returned as null | Medium |
| DEF-003 | POST-004 | Invalid Boolean data type handling | String "yes" converted to true | Medium |
| DEF-004 | POST-005 | Invalid date format handling | Malformed date values returned | High |
| DEF-005 | POST-006 | Invalid date range accepted | Checkout date earlier than check-in date accepted | Medium |
| DEF-006 | PATCH-004 | Invalid booking ID handling | HTTP 405 instead of expected HTTP 404 | Medium |
| DEF-007 | PATCH-005 | Invalid numeric data type accepted during update | totalprice became null and persisted | Medium |

**Severity values are provisional** and should be evaluated against API requirements and business impact.

## Detailed Defect Findings

### DEF-001: Missing Booking Data

- **Test Case:** POST-002
- **Expected Result:** API should handle incomplete booking data appropriately.
- **Actual Result:** HTTP 500 Internal Server Error.
- **Observation:** Incomplete request data resulted in a server error.

### DEF-002: Invalid Numeric Data Type

- **Test Case:** POST-003
- **Expected Result:** API should validate numeric fields.
- **Actual Result:** API returned HTTP 200 and `totalprice` became null.
- **Observation:** Invalid numeric input was not rejected as expected.

### DEF-003: Invalid Boolean Data Type

- **Test Case:** POST-004
- **Expected Result:** API should validate Boolean input.
- **Actual Result:** String value `"yes"` resulted in `depositpaid: true`.
- **Observation:** API appears to perform implicit type conversion.

### DEF-004: Invalid Date Format

- **Test Case:** POST-005
- **Expected Result:** API should reject malformed date formats.
- **Actual Result:** API returned malformed date values, including `0NaN-aN-aN`.
- **Observation:** Date validation and parsing may not properly handle invalid input.

### DEF-005: Invalid Date Range

- **Test Case:** POST-006
- **Expected Result:** Checkout date should not be earlier than check-in date.
- **Actual Result:** API accepted the reversed date range.
- **Observation:** Date range validation may be insufficient.

### DEF-006: Invalid Booking ID

- **Test Case:** PATCH-004
- **Expected Result:** HTTP 404 Not Found.
- **Actual Result:** HTTP 405 Method Not Allowed.
- **Observation:** API returned an unexpected HTTP status when PATCH was sent to a non-existent booking ID. Further verification against the API contract is required.

### DEF-007: Invalid Data Type During Partial Update

- **Test Case:** PATCH-005
- **Expected Result:** API should reject invalid numeric input and preserve the existing booking value.
- **Actual Result:** API returned HTTP 200 and changed `totalprice` to null.
- **Verification:** A subsequent GET request using the same booking ID confirmed that `totalprice` remained null.
- **Observation:** Invalid input was accepted and the resulting value persisted.

---

# Authentication & Environment Configuration

Postman environment variables are used to simplify request configuration.

| Variable | Description |
|---|---|
| `baseUrl` | Restful Booker API base URL |
| `token` | Authentication token generated through AUTH-001 |
| `bookingId` | Booking ID generated through POST-001 |

The token is automatically saved using a Postman test script:

```javascript
const response = pm.response.json();

if (pm.response.code === 200 && response.token) {
    pm.environment.set("token", response.token);
}
```

Authenticated requests use the following Cookie header:

```text
Cookie: token={{token}}
```

**Security Note:** Actual authentication tokens and sensitive environment values must not be uploaded to a public GitHub repository.

---

# How to Run

1. Clone or download this repository.
2. Open Postman.
3. Import the exported Postman collection.
4. Import or create the Restful Booker environment.
5. Configure the `baseUrl` variable.
6. Run AUTH-001 to generate and save an authentication token.
7. Execute the test requests according to their respective collections.
8. Review the response status, response body, and test assertions.
9. Record execution results and document potential defects.

**Note:** Restful Booker is a public shared testing environment. Booking records may be reset or modified independently. Generate a new booking when the referenced booking ID is no longer available.

---

# Conclusion

This project demonstrates practical experience in API testing using Postman, including request configuration, authentication, response validation, positive and negative testing, JavaScript assertions, and defect documentation.

The testing activities also provide hands-on experience in analyzing unexpected API behavior, verifying data persistence, and documenting findings in a structured manner.

---

## Author

**Muhammad Aksal Pratama**

Informatics Engineering Graduate  
Interested in Software Quality Assurance, Manual Testing, and API Testing.

---

## References

- [Restful Booker API](https://restful-booker.herokuapp.com/)
- [Restful Booker API Documentation](https://restful-booker.herokuapp.com/apidoc/index.html)
- [Postman Documentation](https://learning.postman.com/docs/)
