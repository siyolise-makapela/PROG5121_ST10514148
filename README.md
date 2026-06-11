PROG5121 Programming 1A ChatApp POE

ChatApp – Final Application
This part 3 looks at the completed ChatApp and how it should run with all the logins, registrations

## Project Structure

```
ChatApp/
├── src/
│   ├── main/
│   │   └── java/
│   │       ├── Login.java        # User registration, login, cell number validation
│   │       ├── Message.java      # Message creation, validation, hashing, and actions
│   │       └── Main.java         # Application entry point
│   └── test/
│       └── java/
│           ├── LoginTest.java    # JUnit 4 tests for Login class
│           └── MessageTest.java  # JUnit 4 tests for Message class
└── pom.xml                       # Maven build file with JUnit 4 dependency
```

---

## Features

### Login
- Username validation — must contain an underscore and be 5 characters or fewer
- Password complexity check — minimum 8 characters, at least one capital letter, one digit, and one special character
- Cell phone number validation — must include an international dialling code (e.g. `+27...`)
- User registration and login with welcome or error messages

### Message
- Auto-generated unique 10-character alphanumeric message ID
- Message length validation — maximum 250 characters
- Recipient cell number validation — reuses the same logic from `Login`
- Message hash generation — format: `{ID}:{messageIndex}:{FIRSTWORD}{LASTWORD}` (fully uppercase)
- Send action — user chooses to Send, Disregard, or Store each message
- Full message summary printed at the end of the session

---

## POE Test Data

| Field          | Message 1                                        | Message 2                                  |
|----------------|--------------------------------------------------|--------------------------------------------|
| Message Number | 1                                                | 2                                          |
| Recipient      | +27718693002 ✅                                  | 08575975889 ❌ (no international code)     |
| Message Text   | Hi Mike, can you join us for dinner tonight?     | Hi Keegan, did you receive the payment?    |
| Hash Ending    | :0:HITONIGHT                                     | N/A                                        |
| Action         | Send                                             | Disregard                                  |

---

## Running the Application

1. Open the project in **NetBeans**.
2. Right-click the project → **Run**.
3. Follow the console prompts:
   - Enter your first name, last name, username, password, and cell number
   - Log in with your registered credentials
   - Choose how many messages to send
   - For each message: enter a recipient, type your message, then choose Send / Disregard / Store

---

## Running the Tests

**Run all tests:**  
Right-click the project name → **Test**

**Run only MessageTest:**  
Right-click `MessageTest.java` → **Test File**

**Run only LoginTest:**  
Right-click `LoginTest.java` → **Test File**

Results appear in the **Test Results** panel at the bottom of the screen.  
A green tick means the test passed. A red cross means it failed — click it to see the expected vs actual values.

---

## Test Coverage

### LoginTest.java (11 tests)
| Test | What it checks |
|---|---|
| `testCheckUserName_validUsername_returnsTrue` | Valid username passes |
| `testCheckUserName_noUnderscore_returnsFalse` | Missing underscore fails |
| `testCheckUserName_tooLong_returnsFalse` | Username over 5 chars fails |
| `testCheckUserName_validShortUsername_returnsTrue` | Short valid username passes |
| `testCheckPasswordComplexity_validPassword_returnsTrue` | Strong password passes |
| `testCheckPasswordComplexity_tooShort_returnsFalse` | Short password fails |
| `testCheckPasswordComplexity_noCapital_returnsFalse` | Missing capital fails |
| `testCheckPasswordComplexity_noDigit_returnsFalse` | Missing digit fails |
| `testCheckPasswordComplexity_noSpecialChar_returnsFalse` | Missing special char fails |
| `testCheckCellPhoneNumber_validNumber_returnsSuccess` | +27718693002 passes |
| `testCheckCellPhoneNumber_invalidNumber_returnsFailure` | 08575975889 fails |
| `testRegisterUser_validCredentials_returnsSuccess` | Both valid → success message |
| `testRegisterUser_invalidUsername_returnsUsernameError` | Bad username → username error |
| `testRegisterUser_invalidPassword_returnsPasswordError` | Bad password → password error |
| `testLoginUser_correctCredentials_returnsWelcome` | Correct login → welcome message |
| `testLoginUser_wrongPassword_returnsError` | Wrong password → error message |

### MessageTest.java (13 tests)
| Test | What it checks |
|---|---|
| `testCheckMessageLength_validMessage_returnsSuccess` | Under 250 chars passes |
| `testCheckMessageLength_over250chars_returnsFailureWithCount` | Over 250 chars shows count |
| `testCheckMessageLength_exactlyAtLimit_returnsSuccess` | Exactly 250 chars passes |
| `testCheckMessageLength_oneOver_returnsFailureWithCountOf1` | 251 chars shows count of 1 |
| `testCheckRecipientCell_validNumber_returnsSuccess` | Valid cell number passes |
| `testCheckRecipientCell_invalidNumber_returnsFailure` | Invalid cell number fails |
| `testCreateMessageHash_correctFormat_endsWithExpectedWords` | Hash ends with :0:HITONIGHT |
| `testCreateMessageHash_isUppercase` | Full hash is uppercase |
| `testCreateMessageHash_multipleMessages_loopTest` | Loop checks multiple hashes |
| `testCheckMessageID_generatedID_isNotNull` | ID is not null |
| `testCheckMessageID_generatedID_isExactly10Chars` | ID is 10 chars or fewer |
| `testSentMessage_userSelectsSend_returnsCorrectString` | Option 1 → sent message |
| `testSentMessage_userSelectsDisregard_returnsCorrectString` | Option 2 → disregard message |
| `testSentMessage_userSelectsStore_returnsCorrectString` | Option 3 → stored message |

---

## Dependencies

Add this to your `pom.xml` under `<dependencies>`:

```xml
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.13.2</version>
    <scope>test</scope>
</dependency>
```

---

## Important Notes

- `MessageTest.java` and `LoginTest.java` must be placed in `src/test/java`, not `src/main/java`.  
  In NetBeans: right-click the class → **Tools** → **Create/Update Tests** to place it correctly.
- The `sentMessage()` tests use an inner `TestableMessage` class that overrides the Scanner input — this is intentional and avoids console interaction during testing.
- Message IDs are randomly generated on each run, so tests check the ID length and format rather than an exact value.
- The hash for message 1 always ends with `:0:HITONIGHT` regardless of the random ID prefix.

---


