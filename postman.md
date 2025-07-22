# Postman

1. **Creating Environments**

   1.1. Click the gear icon (⚙️) in the top-right corner → **Manage Environments**.

   1.2. Click **Add** to create a new environment.

   1.3. Define key-value pairs (e.g., `base_url`, `access_token`).

   1.4. Save the environment and select it from the environment dropdown.

2. **Using Response Data in Subsequent Requests**

   2.1. **Authentication Request** (capture tokens)

   * **URL**

     ```http
     {{base_url}}/{{auth_endpoint}}
     ```

   * **Request Body (JSON)**

     ```json
     {
       "identifier": "{{user_identifier}}",
       "password": "{{user_password}}"
     }
     ```

   * **Example Response**

     ```json
     {
       "message": "{{response_message}}",
       "access_token": "{{access_token}}",
       "refresh_token": "{{refresh_token}}"
     }
     ```

   * **Test Script**

     ```javascript
     const json = pm.response.json();
     pm.environment.set("access_token", json.access_token);
     pm.environment.set("refresh_token", json.refresh_token);
     ```

   2.2. **Use Tokens in Future Requests**

   * **Header**

     ```http
     Authorization: Bearer {{access_token}}
     ```

3. **Chaining Requests**

   3.1. Use environment variables set in previous requests.

   3.2. Example workflow:

   ```
   1. Authentication → set {{access_token}}
   2. Create Resource → set {{resource_id}}
   3. Update/Delete Resource → use {{resource_id}}
   ```

4. **Common Postman Features for Automation**

   4.1. **Pre-request Scripts**: JavaScript executed before sending a request.

   4.2. **Tests Tab**: Validate responses and set variables.

   4.3. **Collection Runner**: Execute a sequence of requests.