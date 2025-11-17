# ⭐ Step 1 — What is validation in DTOs?

* Validation ensures the data coming from the client is **correct before hitting the database**.
* In Spring Boot, we usually use **Java Bean Validation annotations** (`javax.validation.constraints.*`) on **Request DTOs**.
* Common validations:

  * `@NotNull` → field must not be null
  * `@NotEmpty` → must not be null or empty
  * `@Email` → valid email format
  * `@Size(min=, max=)` → string length limits
  * `@Pattern` → regex validation

---

# ⭐ Step 2 — Applying validation to `UserRequestDTO`

```java
import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import lombok.NoArgsConstructor;

import javax.validation.constraints.Email;
import javax.validation.constraints.NotEmpty;
import javax.validation.constraints.Size;

@Data
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class UserRequestDTO {

    @NotEmpty(message = "Name is required")
    private String name;

    @NotEmpty(message = "Email is required")
    @Email(message = "Email should be valid")
    private String email;

    @NotEmpty(message = "Password is required")
    @Size(min = 6, message = "Password must be at least 6 characters")
    private String password;
}
```

✅ Now, if a client sends invalid data (like empty name or invalid email), Spring can automatically reject it before reaching your service.

---

# ⭐ Step 3 — How validation works in Spring Boot

1. Add `@Valid` on the DTO parameter in your **controller method**:

```java
@PostMapping("/users")
public ResponseEntity<UserResponseDTO> createUser(@Valid @RequestBody UserRequestDTO userRequestDTO) {
    // Service will receive validated DTO
}
```

2. If validation fails, Spring Boot automatically returns **400 Bad Request** with the validation message.

---
# **Common Validation Annotations for DTOs**

### **1. Null / Empty Checks**

* `@NotNull` → Field must not be `null` (but can be empty string)
* `@NotEmpty` → Field must not be `null` **or empty string/collection**
* `@NotBlank` → Field must not be `null`, empty, or whitespace (best for Strings)

### **2. String Length**

* `@Size(min=, max=)` → Restrict string/collection size or length

### **3. Format Checks**

* `@Email` → Must be a valid email format
* `@Pattern(regexp = "…")` → Regex-based validation (custom patterns)

### **4. Numeric Constraints**

* `@Min(value)` → Minimum numeric value
* `@Max(value)` → Maximum numeric value
* `@Positive` → Must be > 0
* `@PositiveOrZero` → Must be ≥ 0
* `@Negative` / `@NegativeOrZero` → For negative values

### **5. Custom Messages**

* You can customize error messages:

```java
@NotEmpty(message = "Name cannot be empty")
@Size(min=3, max=20, message="Name must be 3-20 characters")
```

### **6. Important Tips**

* Use **Request DTOs only**
* Combine multiple annotations for stricter rules
* `@Valid` in controller triggers the validation automatically

---
