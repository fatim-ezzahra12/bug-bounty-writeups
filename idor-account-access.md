# IDOR Vulnerability in User Account Access

## Summary

An Insecure Direct Object Reference (IDOR) vulnerability was identified in a user account endpoint, allowing unauthorized access to other users' data by modifying the user ID parameter.

## Steps to Reproduce

1. Log in to your account

2. Intercept the request using Burp Suite

3. Find a request similar to:
   GET /account?id=123

4. Change the ID parameter to another user:
   GET /account?id=124

5. Send the request

6. Observe that another user's account data is returned

## Impact

* Unauthorized access to sensitive user data
* Potential data leakage (emails, personal info)
* Account takeover scenarios (depending on functionality)

## Recommendation

* Implement proper access control checks
* Validate that the authenticated user owns the requested resource
* Avoid exposing direct object references

## Notes

This vulnerability highlights missing authorization checks on sensitive endpoints and can lead to critical impact depending on the data exposed.
