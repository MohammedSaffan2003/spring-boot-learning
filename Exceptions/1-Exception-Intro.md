# ⭐ Step 1 — Why Exception Handling?

* By default, Spring Boot returns a **big HTML or stack trace** when something goes wrong (like validation failure or resource not found).

* In real-world APIs, clients expect **clean JSON responses** with:

  * `status`
  * `message`
  * (optionally) `timestamp` or `error code`

* Example of a good error response:

```json
{
  "status": 400,
  "message": "Password must be at least 6 characters",
  "timestamp": "2025-11-17T12:45:00"
}
```

---

# ⭐ Step 2 — Global Exception Handler

Spring Boot provides `@ControllerAdvice` + `@ExceptionHandler` for **global exception handling**.

### Basic structure:

```java
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.MethodArgumentNotValidException;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;

import java.util.HashMap;
import java.util.Map;

@ControllerAdvice
public class GlobalExceptionHandler {

    // Handle validation errors
    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<Map<String, String>> handleValidationExceptions(
            MethodArgumentNotValidException ex) {

        Map<String, String> errors = new HashMap<>();
        ex.getBindingResult().getFieldErrors().forEach(error -> {
            errors.put(error.getField(), error.getDefaultMessage());
        });

        return new ResponseEntity<>(errors, HttpStatus.BAD_REQUEST);
    }

    // Handle other exceptions (optional)
    @ExceptionHandler(Exception.class)
    public ResponseEntity<Map<String, String>> handleAllExceptions(Exception ex) {
        Map<String, String> error = new HashMap<>();
        error.put("message", ex.getMessage());
        return new ResponseEntity<>(error, HttpStatus.INTERNAL_SERVER_ERROR);
    }
}
```

---

# ⭐ Step 3 — How it works

1. Client sends invalid `UserRequestDTO` → fails validation
2. Spring throws `MethodArgumentNotValidException`
3. `GlobalExceptionHandler` catches it → converts to **clean JSON**
4. Client receives a structured error instead of HTML or stack trace

---

# ⭐ Step 4 — Key Takeaways

* Always use `@ControllerAdvice` for global exception handling
* Handle **validation errors** separately (`MethodArgumentNotValidException`)
* You can handle **custom exceptions** (like `UserNotFoundException`) the same way
* Keeps your API **clean, consistent, and professional**

---
