# **DTOs (Data Transfer Objects) — Quick Notes**

## **1. What is a DTO?**

* A **Data Transfer Object** is used to transfer data between layers (Controller ↔ Service ↔ Client) without exposing internal database entities.
* Purpose:

  * Avoid exposing sensitive info (e.g., passwords)
  * Decouple API from database structure
  * Provide stable, clean responses

---

## **2. Types of DTOs**

1. **Request DTO** – what the client sends

   * Example: `UserRequestDTO` → name, email, password
2. **Response DTO** – what the server returns

   * Example: `UserResponseDTO` → id, name, email

**Rule:** Never put sensitive info like password in **Response DTO**.

---

## **3. Mapping Entity ↔ DTO**

* **Manual mapping**: write code to copy fields

  ```java
  UserResponseDTO dto = UserResponseDTO.builder()
      .id(user.getId())
      .name(user.getName())
      .email(user.getEmail())
      .build();
  ```
* **Automatic mapping**: use libraries later (ModelMapper, MapStruct)

**Flow**:

```
Database → Entity → Service → DTO → Controller → Client
Client → RequestDTO → Controller → Service → Entity → Database
```

---

## **4. Lombok for DTOs**

* **Reduces boilerplate code** (getters, setters, constructors, builder)
* Common annotations for DTOs:

  * `@Data` → getters, setters, toString, equals/hashCode
  * `@NoArgsConstructor` → empty constructor
  * `@AllArgsConstructor` → constructor with all fields
  * `@Builder` → builder pattern for easy object creation

**Builder advantage**:

* Clear, readable object creation
* No confusion with parameter order
* Works well with many fields

---

## **5. Rules / Best Practices**

* Use DTOs **instead of entities** in controller responses
* Use **Request DTO** for inputs, **Response DTO** for outputs
* Always **exclude sensitive data** from Response DTO
* Manual mapping first → later can use mapping libraries
* DTOs make APIs **clean, safe, and maintainable**

---
